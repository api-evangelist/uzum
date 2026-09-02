---
name: uzum-take-qr-payment-at-pos
description: Take a QR payment at a till with Uzum Fast Pay or Uzum Dynamic QR, read the outcome correctly, and reverse it.
api: Uzum Fast Pay, Uzum Dynamic QR
operations:
  - payment
  - fiscal
  - reversal
  - status
  - create_order
  - check_order
  - cancel_order
generated: '2026-09-02'
method: generated
source: openapi/uzum-fastpay-openapi.yaml, openapi/uzum-dynamicqr-openapi.yaml + https://developer.uzumbank.uz/en/
---

# Take a QR payment at the till

Two products, one host (`https://mobile.apelsin.uz`), opposite directions:

- **Uzum Fast Pay** — the **seller scans the customer's** QR code from the Uzum Bank app.
  Paths under `/api/apelsin-pay/merchant`.
- **Uzum Dynamic QR** — the **customer scans a QR** printed on the receipt or shown on the
  cash-register screen. Paths under `/api/dynamic-qr/merchant`.

## The one thing that will bite you

**Both APIs return HTTP 200 for failures.** From the Fast Pay documentation, verbatim: "All
methods return an HTTP status of 200, even in cases where the request is not successfully
processed."

The outcome is in the body:

- `error_code: 0` and `error_message: null` — success.
- anything else — failure, with the reason in `error_message`.

Any client that branches on the HTTP status will book a declined payment as a completed sale.
Branch on `error_code`.

## Authentication — and its 50-second fuse

The `Authorization` header is a composite signed value matching
`^\d*:(\d{40}):\d*$` — your `merchant_id`, a 40-character hash, and a millisecond timestamp:

```
Authorization: 8461:964bd9d82f6f3c13052f205e92af508cdd7085cc:1727122154352
```

**It expires.** Error `403` means more than 50 seconds elapsed between signing and processing.
Sign each request immediately before sending it; never cache a signed header, and never reuse
one across a retry.

Error `401` covers the rest of the auth surface: header shape wrong, `merchant_id` /
`service_id` / `merchant_service_user_id` inactive, hash mismatch, or `service_id` not belonging
to your partner account.

## Fast Pay flow

1. `payment` (`POST /api/apelsin-pay/merchant/v2/payment`) — scan the customer's QR, submit
   `otp_data` (40 characters or more) and the amount.
2. `fiscal` (`POST /api/apelsin-pay/merchant/payment/fiscal`) — fiscalize the sale.
3. `status` (`POST /api/apelsin-pay/merchant/payment/status`) — resolve any uncertain outcome by
   `payment_id`.
4. `reversal` (`PUT /api/apelsin-pay/merchant/v2/payment/reversal/{orderId}`) — cancel.

Note the versioning: `payment` and `reversal` are on `/v2`, `fiscal` and `status` are
unversioned, on the same contract.

## Dynamic QR flow

1. `create_order` (`POST /api/dynamic-qr/merchant/payment/create`) — returns the payment link to
   render as a QR, of the form
   `https://www.apelsin.uz/open-service?serviceId=…&orderId=…`.
2. `check_order` (`POST /api/dynamic-qr/merchant/payment/status-by-order`) — poll for the result.
3. `fiscal` — fiscalize.
4. `cancel_order` before payment, `reversal` after it.

## Declines worth handling by name

- **416 — card in Safe Mode.** New Uzum Bank users must complete three payments in the app before
  Safe Mode lifts. This is not your error and not the customer's card failing; explain it and
  offer another tender.
- **400 — amount is zero, null or negative; `otp_data` shorter than 40 characters; service
  blocked; card inactive; insufficient funds; invalid or empty `payment_id`.**
- **503 — service not found or inactive.** Configuration; call the account manager.

## Retries

No idempotency header. Resolve every uncertain payment with `status` / `check_order` on the
original `payment_id` or `order_id` before you charge again.
