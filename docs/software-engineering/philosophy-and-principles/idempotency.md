# Idempotency

Properly implementing idempotency for a particular operation, using a working example of a HTTP API.

Issue:

- `POST` and `PATCH` are both non-idempotent (as is `CONNECT` but meh).
- `POST` is often used for _creating_ things.
- We might want to retry, or do other functionality that results in a repetition of the `POST` operation.

Solution: Idempotency!

Add an `Idempotency-Key` header to the request, and include an idempotency key value for dealing with idempotency - as
per [this (draft) RFC](https://datatracker.ietf.org/doc/draft-ietf-httpapi-idempotency-key-header/).

Ensure idempotency is evaluated correctly:

- Idempotency-Key values should be stored (up to the API to determine how long to store for - probably not forever, but
  should be longer than a reasonable retry/duplication should occur... 7 days, maybe?)
- Idempotency-Keys signify a duplicated operation, but they are not the ONLY signifier. The operation itself must also
  be duplicate - e.g. for the `POST` call, the operation is the same if the URL and the HTTP body is the same.
	- If the URL or HTTP body is different but the idempotency-key value is the same, this should not be treated as a
	  duplicate message.
