# Idempotency

## Scenario

**Problem**: We have an API operation that can be "called" (actioned / requested / triggered) and each execution of the
operation produces a different result, even when the same basic inputs are provided. Calling this API may not result in
a response (e.g. due to network outage). Without the response, we are unaware whether the call reached the API nor
whether the operation was executed or not, nor if it was successful or not if it was received and executed. This means
human intervention is required to check if the operation was actually executed or not, before the caller can retry.

**Desired behaviour**: We want to make multiple identical calls to the API operation (e.g. automatic retrying), but have
the API perform only a single execution of the operation. When a result (success or error) is produced by the execution,
all duplicated calls should receive the same result as originally executed.

**Solution**: Implement idempotency! When calling the API operation, we provide an "Idempotency Key" which helps
identify a unique call to the API operation - where the API will treat multiple identical calls made with the same
Idempotency Key as duplicated calls, and thus only execute the operation once. If the caller of the API does not receive
a response, it can retry the API call with the same idempotency key as much as it likes, safe in the knowledge that the
API operation will only be executed once.

> Idempotence is the property of certain operations in mathematics and computer science whereby they can be applied
> multiple times without changing the result beyond the initial application.    
> ~ Wikipedia, 2025

It is important that the API handles idempotency safely and correctly, so that the API caller
can [attempt retries safely](#safe-retries).

## Idempotency in the API

### Storing Keys

Idempotency Keys should be stored in persistent storage (e.g. a database table) for an extended period of time (up to
the API to determine how long to store for - probably not forever, but should be longer than a reasonable
retry/duplication should occur... 7 days, maybe?). This **must** be communicated in the API contract / specification.

### Identifying duplicate API calls

An Idempotency Key helps signify a duplicated attempt at an operation, but it is not the only signifier. A call to an
operation is only a duplicate if ALL the following are true:

- The operation is the same.
- The caller is the same (Unidentified callers should be treated as the same caller).
- The idempotency key is the same.

This table visualises behaviour expected when receiving calls that have some kind of duplication:

| Operation            | Caller                | Idempotency Key      | Is a duplicate call? Should be treated idempotently?                                                                                                        |
|----------------------|-----------------------|----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Same                 | Identified, Same      | Same                 | Yes                                                                                                                                                         |
| Same                 | Unidentified          | Same                 | Yes, all unidentified callers are treated as the same caller.                                                                                               |
| Different            | Any (including same)  | Any (including same) | No, any same callers or idempotency keys are coincidental.                                                                                                  |
| Any (including same) | Identified, Different | Any (including same) | No, any same operations or idempotency keys are coincidental.                                                                                               |
| Any (including same) | Any (including same)  | Different            | No, any same operations or callers are coincidental. If both caller and operation are the same, then it's simply a fresh attempt, not a duplicated attempt. |

> As documented, unidentified callers should be treated as the same caller. However, if you actually allow for
> unidentified callers to call normally non-idempotent operations (creating and patching) this can be quite a critical
> security issue. I would recommend re-considering your architecture and instead use a zero-trust architecture and
> require identifiable callers.

### Responding correctly

When the API operation produces a result, the result should be stored and linked to the Idempotency Key, so that the
API provider can easily fetch and return the same result when subsequent duplicated calls are made. This is typically
done by storing the result alongside the Idempotency Key in the same persistent storage.

When errors occur during execution, an error result is produced. Duplicated calls should receive the same error result.
Idempotency is a solution for when callers are unaware of an APIs response (and subsequently time out), not for when an
API provides an error response. Not storing and returning the same error response means that subsequent duplicated
calls will result in multiple executions of the API operation, thus voiding the safety of idempotency and creating
unnecessary and unsafe variability in the API's behaviour, and ultimately: breaking the actual property that is
"idempotence"!

### Handling concurrency

If the API can perform operations concurrently, duplicate calls could be made in parallel before the execution of the
first received call has finished. To guarantee idempotency, the following must be done:

- The API should check if an Idempotency Key exists and store it if it doesn't as soon as a call is received, before any
  operation execution begins. This "check and store" operation must be a blocking action, to prevent concurrent
  processes checking and storing at the same time (e.g. achieved by locking a database table whilst performing the check
  and store).
- When the API operation produces a result, duplicate received calls must wait until the first received call is finished
  and a result is produced before receiving the result.

### Example: HTTP API

For an HTTP API, the `POST` and `PATCH` methods are non-idempotent / non-safe by design, and so need to have idempotency
implemented by the API provider.

When evaluating whether an HTTP request is a duplicate:

- The operation is identifiable by the combination of the HTTP Method, URL path and Request Body used.
- The caller is identifiable by the value of the `Authorization` header (or a value within the contents of this header),
  or possibly other client ID.
- The Idempotency Key should be found in the `Idempotency-Key` header, as
  per [this draft RFC](https://datatracker.ietf.org/doc/draft-ietf-httpapi-idempotency-key-header/).

The HTTP API provider should store the HTTP response code, any appropriate HTTP headers and the HTTP response body for
returning when responding to duplicate HTTP requests (even in the case of errors), to ensure proper idempotent responses
are given (see [above](#responding-correctly)).

An HTTP API typically allows operations to be performed concurrently - e.g. individual requests are handled by separate
threads. As described [above](#handling-concurrency), this means duplicate HTTP requests made in parallel must be
held/paused whilst execution of the first receive request completes before an HTTP response can be given. Some may make
a case for responding with HTTP 425 (Too Early) for concurrent requests whilst the original request is being processed,
however I recommend not doing this and simply responding with the same response to maintain pure and predictable
idempotency.

If an HTTP API responds with an error response, the HTTP client may want to retry the call, this should be done with a
new Idempotency Key.

If an HTTP client does not receive a response, the HTTP client will eventually time out and may want to retry the call,
this should be done with the same Idempotency Key as originally used.

### Example: Message bus consumer

A "message bus" here refers to any asynchronous message queue, event bus, pub/sub (etc.) implementation.

A message bus may not be able to guarantee "exact-once delivery" to consumers, and so consumers need to have idempotency
implemented in order to mitigate receiving duplicate messages from the message bus. Message bus systems also often
require consumers to acknowledge or not (ACK or NACK) whether a message has been received/consumed/processed. If no ACK
response is received, the message bus usually assumes a NACK and will try delivering again.

When evaluating whether a message is a duplicate, unfortunately message buses are often implemented in different ways.
The "operation" could be a topic or a message type, the "caller" (the producer of the message) may or may not be
identifiable, and messages in the message bus may or may not have IDs.  
If the message bus uses the [CloudEvents spec](https://github.com/cloudevents/spec) (I recommend this):

- The operation value of the `type` attribute.
- The caller is the value of the `source` attribute.
- The Idempotency Key is message ID value of the `id` attribute.

Consumers of message buses may or may not process received messages concurrently. In distributed web service
architectures, consumers are often replicated for resiliency and thus can process messages concurrently - as such,
duplicate messages consumed in parallel must have some protection in place to ensure that multiple operation executions
do not occur. The Inbox pattern and Dead Letter Queues are two ways to handle this.

> The Inbox pattern is where consumed messages are persisted to an "inbox" (typically a database) and then ACKed (no
> real operational processing yet done). Constraints in the Inbox should ensure message uniqueness, so if a unique
> message is already in the Inbox, a duplicate will not be allowed and the message can then simply be ACKed. The
> consumer is then free to process a clean Inbox with no duplicates in its own time.

> I tend to prefer Inboxes over DLQs, as the consumer tends to have a database anyway, and even if it didn't, I'd rather
> set up and deal with database infrastructure and gain indefinite persistence and the decoupling advantages of an
> Inbox, than risk losing data due to DLQ message retention but solely rely on message bus infrastructure for
> processing.

## Safe retries

### Simple retry logic

Once an API has [safely implemented idempotency](#idempotency-in-the-api), a caller can integrate with the API and
retrying according to the logic of idempotency:

- Perform the operation, including an idempotency key (`ABC`).
- If a response was received, and it was a success, then the caller can continue and no retries are necessary.
- If a response was received, and it was a failure, then the caller knows that retrying the request with the same
  idempotency key (`ABC`) as before will result in the same error response. If the caller wishes to retry, it must do so
  with a new idempotency key (`XYZ`).
- If no response was heard, the caller has no idea whether the request was received and processed by the API provider.
  If the caller wishes to retry, it must do so with the same idempotency key as before (`ABC`).

### Multiple layers of retries

In some systems you may have multiple "layers" of retries, for example: an HTTP client can retry, and the code
triggering the HTTP client can retry as well. This can be quite dangerous to implement if dealing with idempotency.
Let's name the caller the "Caller", and the code triggering the caller the "Trigger".

When the Caller receives no API response, it can retry with the same idempotency key. Eventually, it might never receive
an API response, and have exhausted all its retries. The Trigger might then choose to retry again, but if the
idempotency key is not saved between these retries, then the Caller could make a fresh call to the API with a new
idempotency key, even though it never received an API response. This is bad.

There are two ways to solve this problem:

- Return the Idempotency Key to the Trigger
	- When the Caller exhausts the retries due to error API responses, it fails but does not return the idempotency key
	  to the Trigger. The Trigger can then retry and a new idempotency key is used in the Caller (as normal).
	- When the Caller exhausts the retries due to no API responses, it fails and returned the idempotency key to the
	  Trigger. The Trigger can then try, but must pass the idempotency key back to the Caller for the Caller to use, to
	  ensure calls continue to be idempotent.
- The Caller communicates to the Trigger that the call is non-retryable, and manual intervention is needed.
	- When the Caller exhausts all retries due to error API responses, it fails with an error type that allows for
	  retries. The Trigger can then retry and a new idempotency key is used in the Caller (as normal).
	- When the Caller exhausts all retries due to no API responses, it fails with an error type that tells the Trigger
	  to not attempt a retry as it is unsafe. The Trigger must then not attempt a retry.
	- This effectively the same behaviour that should be in place when no API response was given, from before
	  idempotency was used, but retries have at least been attempted.

> In this case, I prefer option two, as it's much less complex to implement, and I'm simply happy that retrying has
> been attempted before manual intervention was necessary.  
> If non-retryable errors are consistently being produced however, then I will consider implementing option one, though
> I will also consider using a different API, assuming that is possible.

### Retrying is precarious

This makes retrying quite a precarious action, but... that's the way that idempotency works, to give us the ability to
perform automatic retries / multiple identical calls when no API response is given.

Retrying when an error response is returned was already possible without idempotency.

If you are thinking that retrying is now too precarious, then you should simply never retry if you don't receive a
response, never interact with idempotency and always require manual intervention.  
If you are thinking of retrying with the same idempotency key when receiving an error "for safety", this will always
garner the error same response as the original attempt, and will result in your maximum retry count being exhausted, and
manual intervention will be required anyway - which renders idempotency effectively useless, as the purpose is to
facilitate safe retries in the event of no API response.
