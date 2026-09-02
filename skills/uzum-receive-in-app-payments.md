---
name: uzum-receive-in-app-payments
description: Implement the five Uzum Merchant API webhooks so customers can pay for your service from inside the Uzum Bank mobile application.
api: Uzum Merchant API
operations:
  - check
  - create
  - confirm
  - reverse
  - status
generated: '2026-09-02'
method: generated
source: openapi/uzum-merchant-openapi.yaml + https://developer.uzumbank.uz/en/merchant
---

# Receive payments from inside the Uzum Bank app

This one is inverted from every other Uzum API: **you build the server and Uzum calls you.** The
contract (`openapi/uzum-merchant-openapi.yaml`) is an OpenAPI 3.1.0 document with **zero paths** —
the whole surface is in the top-level `webhooks` object, so any tooling that counts operations
from `paths` will tell you this API is empty. It is not.

- Transport: HTTPS POST, `application/json`, to endpoints **you** host.
- Auth: HTTP Basic, credentials issued by Uzum Bank. Verify them on every call.
- Payment flow diagram: https://developer.uzumbank.uz/img/merchant-api/en-merchant-payment-flow.svg

## The five handlers

1. **`/check` — Verifying Payment Possibility.** The customer picked your service in the Uzum
   Bank app and entered their data (account number, phone number, order number); it arrives in
   the `params` object. Validate it and return `status: OK`, or `status: FAILED` with an
   `errorCode`. **This call must not create anything** — it is asked speculatively, and it may be
   asked more than once.
2. **`/create` — Creating Payment Transaction.** Carries `transId` (Uzum's transaction id),
   `amount` and the payment parameters. Create the transaction on your side against `transId` and
   return `status: CREATED`, or `status: FAILED` with an `errorCode`. **Store `transId` as a
   unique key** — this is where your duplicate protection lives, because Uzum publishes no
   idempotency header.
3. **`/confirm` — Confirming Payment Transaction.** Uzum has already debited the customer by the
   time this arrives. Deliver the service, move the transaction to its final state, and return
   `status: CONFIRMED`. If you return `FAILED` here, the customer has paid and not been served —
   so make delivery idempotent on `transId` and return `CONFIRMED` on a repeat.
4. **`/reverse` — Cancelling Payment Transaction.** Unwind. Must be safe to call twice.
5. **`/status` — Checking Payment Transaction Status.** Reconciliation. Return the current state
   for `transId` and nothing else; never mutate here.

## Response discipline

Every handler documents just two responses: `200` and `400`. The business outcome rides in the
`status` field of a 200 body, not in the HTTP code:

- `/check` → `OK` | `FAILED`
- `/create` → `CREATED` | `FAILED`
- `/confirm` → `CONFIRMED` | `FAILED`

Return `400` only for a malformed request. Returning `400` for a business rejection loses the
`errorCode` Uzum needs to show the customer something useful.

## Idempotency is your job

Uzum will retry. The contract gives you no idempotency key and no delivery-retry policy, so key
every handler on `transId` and `serviceId` — the two fields that appear across all fifteen
component schemas — and make each one safe to call any number of times.
