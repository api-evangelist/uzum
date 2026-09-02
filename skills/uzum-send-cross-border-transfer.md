---
name: uzum-send-cross-border-transfer
description: Quote, register, OTP-confirm and — where still possible — cancel an international money transfer through Uzum CrossBorder Transfer or Remit Core.
api: Uzum CrossBorder Transfer, Remit Core
operations:
  - convert
  - checkCredit
  - confirmCredit
  - checkDebit
  - resendOTP
  - confirmDebit
  - cancelDebit
  - transferStatus
  - getReceiverCardsByPhone
  - getReceiverBanksList
  - registerTransfer
  - processTransfer
  - getStatus
  - cancel
  - resendOtp
  - getBanks
  - getBalance
generated: '2026-09-02'
method: generated
source: openapi/uzum-crossborder-openapi.yaml, openapi/uzum-remitcore-openapi.yaml + https://developer.uzumbank.uz/en/
---

# Send a cross-border transfer with Uzum

Uzum runs **two** cross-border products and they are not the same API. Pick one before you write
any code.

| | CrossBorder Transfer | Remit Core |
|---|---|---|
| Base | `https://crossborder.transfer.uz` | `https://remit-core.ipt-merch.com` (**test only**) |
| Auth | HTTP Basic | `X-Api-Key` (UUID) |
| Production network | public | **IPSec tunnel + IP allow-list required** |
| Direction | to and from Uzbekistan, plus service payments | CREDIT (primary) and DEBIT (limited) |
| Prefix | `/cbt/v1` | `/v1` and `/info/v1` |

Both operate only where Uzum has enabled the direction in your contract. Error **10006**
("this method is not available") means the operation class was not agreed — it is a contract
question, never a retry.

## CrossBorder Transfer — money leaving Uzbekistan

1. **Quote.** `convert` returns the current rate. It creates nothing.
2. **Register.** `checkDebit` runs the pre-checks and sends an OTP. Blacklist screening happens
   here: **10201** is the sender failing, **10202** the recipient. Neither is retryable.
3. **Resend the OTP** with `resendOTP` if the customer did not receive it.
4. **Confirm.** `confirmDebit`. Once this succeeds the transfer is settled.
5. **Cancel** with `cancelDebit` — available only while the transfer is registered and
   unconfirmed. After `confirmDebit` there is no documented reversal path.
6. **Reconcile** with `transferStatus`, and `transferList` / `getAccountOperations` /
   `getClosingBalance` for the registry and balance.

## CrossBorder Transfer — money arriving in Uzbekistan

1. Resolve the recipient: `getReceiverCardsByPhone` (by phone) or `getReceiverCardsByPAN` (by
   card). These return the cardholder name — use it to show the sender who they are paying
   before you move money. `getReceiverBanksList` lists the recipient's banks.
2. `checkCredit` then `confirmCredit`.
3. **10203** means no recipient card was found for the data supplied.

## Remit Core

1. Optionally `convert` for a quote, and `getBanks` when crediting by phone number — the phone
   instrument requires a `bankLabel` short code alongside it. The code-to-name mapping is
   published as an XLSX download, not as an API resource, so cache it.
2. `registerTransfer` → `processTransfer` → `confirm` (with `resendOtp` if needed).
3. `cancel` while the transfer is registered and not yet processed. `getStatus` is the authority
   on whether that window is still open.
4. `getBalance` for the partner account.

## The rule that matters most

**Your transfer id is your only safety net.** Both products key on a partner-supplied id
(`externalTransferId`, `externalPaymentId`). A duplicate is *rejected* — CrossBorder error
**10211** "Transfer already exists", **20201** "Payment already exists" — it is **not** replayed
back to you with the original result.

So on a 500, a 504 or a network timeout: **call the status method with the id you already used.**
Never generate a fresh id and re-register. There is no `Idempotency-Key` header on either API, and
re-registering is how you send the money twice.

## Testing

- CrossBorder test host: `crossborder-transfer.ub.ufintech.uz`, credentials from the Uzum Bank
  manager. Uzum publishes English test-case PDFs and a Postman collection, plus separate mock and
  real-integration scripts for cross-border service payments. Contact the manager before running
  the mock cases.
- Remit Core: the internet-reachable host **is** the test environment. Ask for the test
  `X-Api-Key`; a CREDIT Postman collection is published.

See `sandbox/uzum-sandbox.yml` for every URL.
