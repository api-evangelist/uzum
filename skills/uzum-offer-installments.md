---
name: uzum-offer-installments
description: Offer Uzum Nasiya installment (BNPL) financing at checkout — check the buyer's limit, quote the plan, create the contract and activate it by SMS.
api: Uzum Nasiya Partner API
operations:
  - checkBuyerStatus
  - calculateOrder
  - createOrder
  - sendContractSmsCode
  - verifyContractSmsCode
  - confirmContract
  - checkContractStatus
  - cancelContract
generated: '2026-09-02'
method: generated
source: openapi/uzum-nasiya-openapi.yaml + https://developer.uzumbank.uz/en/nasiya
---

# Offer Uzum Nasiya installments

Uzum Nasiya is consumer installment lending. A partner sells the goods; Uzum finances them.

- Base URL: `https://merchants-api.uzumnasiya.uz`
- Auth: `Authorization: Bearer <token>`
- **Two path prefixes in one contract**: the contract methods are on `/api/v1`, the SMS
  activation methods are on a bare `/v3`. This is not a typo in the spec — build both.
- The reference is published in Russian only.

## Steps

1. **Check the buyer.** `checkBuyerStatus` (`POST /api/v1/buyers/check-status`) returns whether
   the customer is registered with Uzum Nasiya and what limit they have. If they are not
   registered, send them through the Uzum Nasiya WebView registration flow first — registration
   is a WebView journey, not an API call you can make on their behalf.
2. **Quote the plan.** `calculateOrder` (`POST /api/v1/orders/calculate`) prices the basket into
   installment terms and returns the tariff and the per-product breakdown. This creates nothing —
   call it as often as the customer changes the basket, and show them the result before you
   commit them to anything.
3. **Create the contract.** `createOrder` (`POST /api/v1/orders`). Carry your own
   `ext_order_id`; that is your handle on the contract.
4. **Activate by SMS.** `sendContractSmsCode` (`POST /v3/buyers/send-code-sms`) then
   `verifyContractSmsCode` (`POST /v3/buyers/check-code-sms`). This is the customer's consent
   step — never automate around it.
5. **Confirm.** `confirmContract` (`POST /api/v1/contracts/confirm`) after you have handed over
   the goods.
6. **Reconcile.** `checkContractStatus` (`POST /api/v1/contracts/check-status`).

## Taking it back

`cancelContract` (`POST /api/v1/contracts/cancel`) is a **full** cancellation — the endpoint is
documented as "Полная отмена договора". There is no partial cancellation and **Uzum publishes no
time window**, so do not tell a customer how long they have to cancel. If you need to know the
deadline, get it from your account manager in writing.

## Error handling

Every operation documents 200, 400, 401 and 403. A 403 on this API most often means the operation
class is not enabled for your partner agreement rather than that the buyer is ineligible —
buyer ineligibility comes back from `checkBuyerStatus` as data, not as an HTTP error.

## Retries

There is no idempotency header. `createOrder` is the dangerous call: on a timeout, resolve with
`checkContractStatus` using your `ext_order_id` before creating anything else.
