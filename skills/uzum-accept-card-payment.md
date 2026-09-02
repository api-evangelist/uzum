---
name: uzum-accept-card-payment
description: Take a card payment on a website or in an app through Uzum Checkout, confirm the outcome, and refund or reverse it correctly.
api: Uzum Checkout
operations:
  - register_payment_api_v1_payment_register_post
  - get_order_status_api_v1_payment_getOrderStatus_post
  - get_operation_state_api_v1_payment_getOperationState_post
  - complete_api_v1_acquiring_complete_post
  - refund_api_v1_acquiring_refund_post
  - reverse_api_v1_acquiring_reverse_post
generated: '2026-09-02'
method: generated
source: openapi/uzum-checkout-openapi.yaml + https://developer.uzumbank.uz/en/checkout
---

# Accept a card payment with Uzum Checkout

Uzum Checkout accepts Visa, Mastercard, Uzcard and Humo. Every operation is a POST under
`/api/v1`. **Uzum publishes no base URL for Checkout** — take it from the account manager who
issued your terminal, and do not infer one from the domain.

## Before you start

- You need two headers on every request: `X-Terminal-Id` (your terminal identifier) and
  `X-API-Key`. Both are assigned by Uzum Bank; there is no self-service key.
- Set `Content-Language` to `ru-RU`, `uz-UZ` or `en-EN` to control the payment form language.
- Decide one-step or two-step. One-step authorizes and captures together and is the only mode
  that supports auto-fiscalization. Two-step authorizes now and captures with `complete`.
  Error 3004 means the terminal does not have that mode enabled — that is a configuration
  request to your account manager, not a retry.

## Steps

1. **Register the payment.** Call `register_payment_api_v1_payment_register_post` with the
   amount, an ISO 4217 currency code, and your own order reference. If auto-fiscalization is
   enabled on your terminal, send the cart here: each item with its SKU, packaging code and VAT
   rate. Uzum returns a payment identifier and the form URL.
2. **Send the customer to the form.** The iframe or webview posts a JSON message back to the
   parent: `{status, action, errorCode, payment_id?}` where `status` is `SUCCESS`, `CANCEL` or
   `ERROR` and `action` is always `close`. **Treat this message as a hint, not as truth** — close
   the frame on it, then confirm server-side in step 3.
3. **Confirm the outcome server-side.** Call
   `get_order_status_api_v1_payment_getOrderStatus_post`. The payment state machine is
   `REGISTERED → COMPLETED | DECLINED`, and `REFUNDED` after a refund. Use
   `get_operation_state_api_v1_payment_getOperationState_post` for the state of an individual
   operation.
4. **Capture, if two-step.** Call `complete_api_v1_acquiring_complete_post`.

## Reading a decline

A decline arrives as an `errorCode` in the 3xxx class. Split them:

- **Retryable by the customer** — 3008 insufficient funds, 3012 transaction limit exceeded,
  3013 amount exceeds limit. Tell the customer and let them try again.
- **Do not retry** — 3009 declined by antifraud, 3010 sanctions or other restriction, 3011
  declined by the issuing bank, 3015 card scheme prohibited. Offer a different instrument.
- **Your bug or your configuration** — 3002 invalid amount/currency/fee, 3003 invalid cart,
  3007 incorrect MCC, 3014 MCC not in the allowed list, 3001/3023 terminal or partner not found.
  Fix the request or call the account manager; retrying unchanged will fail identically.

The full registry is in `errors/uzum-decline-codes.yml`.

## Taking it back

- **Before capture (two-step):** `reverse_api_v1_acquiring_reverse_post`.
- **After capture, full or partial:** `refund_api_v1_acquiring_refund_post`.
- Both windows are bounded but **Uzum does not publish the duration**. You learn the window has
  closed from error 3020 (cancellation timeout) or 3022 (refund timeout). Do not promise a
  customer a refund window Uzum has not stated.
- Error 3021 means the refund amount exceeds what was captured. Error 3019 is the same for a
  reversal.

## Retries

There is no `Idempotency-Key` header on this API. On a 500 or a network timeout, **do not
register a second payment** — call `get_order_status_api_v1_payment_getOrderStatus_post` with
your original order reference and act on what it says.

## Testing

Published test cards (verbatim from Uzum):

- HUMO `9860 0901 0121 9724`, exp `10/26`, 3-DS `777777`
- UzCard `8600 3129 2957 7175`, exp `09/26`, 3-DS `777777`

Set `force3ds` on the register call to force the 3-D Secure path. `3040` means 3DS is not
supported for that card, `3051` and `3057` are 3DS failures.
