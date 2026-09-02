---
name: uzum-fiscalize-a-receipt
description: Submit a sale or refund receipt to the Uzbekistan tax authority through Uzum Fiscalization, including the mandatory product labels, and retrieve the receipt link.
api: Uzum Fiscalization
operations:
  - fiscal_receipt_generation_fiscal_receipt_generation_post
  - fiscal_receipt_refund_fiscal_receipt_refund_post
  - get_receipt_url
  - save_qr_code_url_save_qr_code_url_post
  - health_health_get
generated: '2026-09-02'
method: generated
source: openapi/uzum-fiscalization-openapi.yaml + https://developer.uzumbank.uz/en/fiscalization
---

# Fiscalize a receipt with Uzum

Fiscalization is a legal obligation in Uzbekistan, not a feature. Uzum submits your receipts to
the State Tax Committee (GNC) through a Fiscal Data Operator (OFD) and returns a fiscal receipt
link.

- Production host: `https://ofd-key.inplat-tech.com`
- Test host: `https://test-ofd.ipt-merch.com`
- Auth: an API key issued per partner by the Uzum development team. **The test and production
  keys are different.**
- Version prefix: `/v2`

**Check first whether you need this API at all.** If you take payment through Uzum Checkout on a
one-step payment, ask your account manager to switch on auto-fiscalization: Uzum then fiscalizes
for you and you send the cart on `register` instead of calling this API. Auto-fiscalization is
off by default.

## Steps

1. **Fiscalize the sale.** POST `/v2/receipt`
   (`fiscal_receipt_generation_fiscal_receipt_generation_post`) with the receipt: an `items`
   collection where each item carries its SKU, packaging code and VAT rate, plus commission info
   where it applies. A `202` means the receipt was accepted for asynchronous submission — it is
   not yet a fiscal receipt.
2. **Get the receipt link.** GET `/v2/receipt/{operation_id}/receipt_url` (`get_receipt_url`).
   Expect to poll: the contract models a distinct "waiting for receipt URL" response, so the link
   is not available the instant the receipt is accepted.
3. **Refund.** POST `/v2/refund_receipt`
   (`fiscal_receipt_refund_fiscal_receipt_refund_post`) with the same item discipline. This
   reverses the **tax record**. It does not move money — reverse the payment on Checkout, Fast Pay
   or Dynamic QR separately.
4. **QR payment receipts.** POST `/v2/qr_payment` (`save_qr_code_url_save_qr_code_url_post`)
   submits QR-payment receipt data to the tax authority. This one requires prior approval from the
   Uzum development team before it will work.
5. **Health.** GET `/health/` is the only health endpoint Uzum publishes on any of its nine APIs.

## Product labels — the thing that will break you

Since **1 March 2024**, Uzbek law requires a digital product label on receipts for:

1. Tobacco products
2. Alcoholic beverages (except beer and beer-based drinks)
3. Beer products
4. Household appliances and electronics
5. Pharmaceuticals
6. Water and soft drinks

A `labels` array was added to each entry in `items` for this. **If you omit labels for a product
in one of those categories, the service returns an error and the receipt is not fiscalized** —
which means you have taken money and have no fiscal receipt. Validate the category-to-label
mapping before you submit, not after.

## Callbacks

If the tax authority does not answer in real time, Uzum can call you back with the result. Ask
your account manager to enable it and give them the URL. **Uzum does not publish the callback
payload shape**, so agree it with them in writing before you build the handler.

## Getting to production

Uzum gates promotion on a manual review: run the documented test scenarios on
`test-ofd.ipt-merch.com` (multi-item receipts, discounted receipts, marketplace-voucher receipts,
for both sale and refund), then post the generated receipt links in the Uzum Telegram chat. The
production API key is issued in that chat after they confirm.

## Discounts

Two discount fields behave differently and the difference is fiscal:

- `items.discount > 0` — a normal discount, subtracted from the total.
- `items.voucher > 0` — a marketplace discount, **not** subtracted from the total amount.
