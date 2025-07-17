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
- An Idempotency-Key helps signify a duplicated attempt at an operation, but it is NOT the only signifier. An attempt at
  an operation is only a duplicate if ALL the following are true:
	- The operation is the same - For a `POST` request, this is if the URL and HTTP body is the same.
	- The caller is the same - For a `POST` request, check Authentication information. All unidentified callers should
	  be treated as the same caller.
	- The idempotency key is the same.
