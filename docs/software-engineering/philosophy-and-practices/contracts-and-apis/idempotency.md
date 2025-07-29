# Idempotency

Scenario: We have an API operation that can be "called" (actioned / requested / triggered) and each execution of the
operation produces a different result, even when the same basic inputs are provided. Calling this API may not result in
a response (e.g. due to network outage). Without the response, we are unaware (as consumers) whether the call reached
the API and whether it was successful or not.

Desired behaviour: We want to make multiple identical calls to the API operation, but have the API perform a single
execution of the operation. When a result (success or error) is produced by the execution, all duplicated calls should
receive the same result as originally executed.

Solution: Implement idempotency! When calling the API operation, we provide an "Idempotency Key" which represents a
unique call to the operation - where the API will treat multiple identical calls made with the same Idempotency Key as
duplicated calls, and thus only execute the operation once.

It is important that the API handles idempotency safely and correctly, so that the API consumer
can [attempt retries safely](#safe-consumer-retries).

## Evaluating Idempotency

Idempotency Keys should be stored for an extended period of time (up to the API to determine how long to store for -
probably not forever, but should be longer than a reasonable retry/duplication should occur... 7 days, maybe?). This
must be communicated in the API contract / specification.

An Idempotency-Key helps signify a duplicated attempt at an operation, but it is NOT the only signifier. A call to an
operation is only a duplicate if ALL the following are true:

- The operation is the same.
- The caller is the same - i.e. Check Authentication information. Unidentified callers should be treated as the same
  caller.
- The idempotency key is the same.

If the operation or caller is different for two or more calls with the same idempotency key, then this must be treated
as a coincidence and not as a duplicate call.

When the API operation produces a result, the result should be stored and linked to the Idempotency Key, so that the
API provider can easily fetch and return the same result when subsequent duplicated calls are made. This is typically
done by storing the result alongside the Idempotency Key.

When errors occur during execution, an error result is produced. Duplicated calls should receive the same error result.
Idempotency is a solution for when clients are unaware of an APIs response (and subsequently time out), not for when an
API provides an error response.

If the API can perform operations concurrently, duplicate calls could be made in parallel before the execution of the
first received call has finished. To guarantee idempotency, the following must be done:

- The API should check if an Idempotency Key exists and store it if it doesn't as soon as a call is received, before any
  operation execution begins. This "check and store" operation must be a blocking action, to prevent concurrent
  processes checking and storing at the same time (e.g. achieved by locking a database table whilst performing the check
  and store).
- When the API operation produces a result, duplicate received calls must wait until the first received call is finished
  and a result is produced before receiving the result.

## HTTP API

For an HTTP API, the `POST` and `PATCH` methods are non-idempotent / non-safe by design, and so need to have idempotency
implemented by the API provider.

When evaluating whether an HTTP request is a duplicate:

- The operation is identifiable by the URL path used.
- The caller is identifiable by the value of the `Authorization` header (or a value within the contents of this header),
  or other client ID.
- The Idempotency Key should be found in the `Idempotency-Key` header, as
  per [this draft RFC](https://datatracker.ietf.org/doc/draft-ietf-httpapi-idempotency-key-header/).

HTTP is a request/response protocol, meaning a result MUST be provided. The HTTP API provider should therefore store the
HTTP response code, any appropriate HTTP headers and the HTTP response body for returning when responding to duplicate
HTTP requests.

An HTTP API typically allows operations to be performed concurrently - e.g. individual requests are handled by separate
threads. As described above, this means duplicate HTTP requests made in parallel must be held/paused whilst execution of
the first receive request completes before an HTTP response can be given.

If an HTTP API responds with an error response, the HTTP client may want to retry the call, this should be done with a
new Idempotency Key.

If an HTTP client does not receive a response, the HTTP client will eventually time out and may want to retry the call,
this should be done with the same Idempotency Key as originally used.

## Message bus consumer

A "message bus" here refers to any asynchronous message queue, event bus, pub/sub (etc.) implementation.

A message bus may not be able to guarantee "exact-once delivery" to consumers, and so consumers need to have idempotency
implemented in order to mitigate receiving duplicate messages from the message bus. Message bus systems also often
require consumers to acknowledge or not (ACK or NACK) whether a message has been received/consumed/processed. If no ACK
response is received, the message bus assumes a NACK and will try delivering again.

Message buses are often implemented in different ways. The "operation" could be a topic or a message type, the "caller"
(the producer of the message) may or may not be identifiable, and messages in the message bus may or may not have IDs.
If the message bus uses the [CloudEvents spec](https://github.com/cloudevents/spec) (I recommend this):

When evaluating whether a message is a duplicate:

- The operation value of the `type` attribute.
- The caller is the value of the `source` attribute.
- The Idempotency Key is message ID value of the `id` attribute.

Consumers of message buses may or may not process received messages concurrently. In distributed web service
architectures, consumers are often replicated for resiliency and thus can process messages concurrently. As such,
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

## Safe consumer retries

Once an API has safely implemented idempotency (see above), a consumer can integrate with the API sending idempotency
tokens and retrying according to the logic of idempotency.

A consumer can integrate with the API in the following manner:

- Perform the operation, including an idempotency key (ABC).
- If a response was received, and it was a success, then the consumer can continue and no retries are necessary.
- If a response was received, and it was a failure, then the consumer knows that retrying the request with the same
  idempotency key (ABC) as before will result in the same error response, as it will have been cached. If the consumer
  wishes to retry, it must do so with a new idempotency key (XYZ).
- If no response was heard, the consumer has no idea whether the request was received and processed by the API provider.
  If the consumer wishes to retry, it must do so with the same idempotency key as before (ABC).

This makes retrying quite a delicate / precarious action.

If you want to build a resilient system (e.g. eventual consistency with automatic retrying), then you should attempt
automatic retries, and follow the consumer idempotency logic above.

If you are thinking that retrying is too precarious, then you should simply never retry and always require manual
intervention. Retrying with the same idempotency key in every scenario for safety will always garner the same response,
and thus in the case of failures, result in your maximum retry count being exhausted, and manual intervention is then
required anyway (which incidentally renders idempotency effectively useless, as the original purpose is to facilitate
safe retries in the event of no API response).
