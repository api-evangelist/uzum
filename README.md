# Uzum

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Uzum is an Uzbekistan-based digital ecosystem headquartered in Tashkent that bundles e-commerce, fintech and digital banking into a single group: Uzum Market (the country's largest online marketplace), Uzum Tezkor (rapid food and grocery delivery), Uzum Bank (a licensed digital bank), and Uzum Nasiya (consumer BNPL / installment lending). Its developer-facing API surface is published by Uzum Bank at developer.uzumbank.uz, where nine OpenAPI contracts and a JSON-RPC 2.0 Banking-as-a-Service hub cover card acquiring (Uzum Checkout), QR and cash-register payments (Uzum Fast Pay, Dynamic QR), tax receipt fiscalization to the Uzbek State Tax Committee (Uzum Fiscalization), inbound and outbound cross-border money transfer (Uzum CrossBorder Transfer, Remit Core), installment contract origination (Uzum Nasiya Partner API), in-app merchant billing (Merchant API webhooks) and FX rate quoting (RateKeeper). Uzum Market additionally operates a seller-cabinet API at api-seller.uzum.uz for FBO/FBS order, inventory and price synchronization, which is credential-gated. All Uzum Bank API programs are onboarded through an account manager rather than self-service signup.

**Areas:** Company · Payments · Banking · Financial Services · E-Commerce · Marketplace · Money Transfer · Remittances · Buy Now Pay Later · Acquiring · QR Payments · Fiscalization

## Uzum on APIs.io

Everything profiled for Uzum is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/uzum/](https://apis.io/providers/uzum/)** — or go straight to one of the APIs or properties below.

### APIs (19)

- [Uzum BaaS Payment Hub](https://apis.io/apis/uzum/uzum-payment-hub-baas/) — Banking-as-a-Service JSON-RPC 2.0 API that lets partners embed payments, transfers and account and card operations inside Uzbekistan's national payment…
- [Uzum Market Seller API](https://apis.io/apis/uzum/uzum-market-seller/) — Seller-cabinet API for Uzum Market used to synchronise marketplace orders, inventory and prices for both Fulfilment by Operator (FBO) and Fulfilment by…
- [Uzum API MFO API](https://apis.io/apis/uzum/uzum-api-mfo-api/) — The API MFO API from Uzum — 8 operation(s) for api mfo.
- [Uzum Auxiliary Methods API](https://apis.io/apis/uzum/uzum-auxiliary-methods-api/) — The Auxiliary Methods API from Uzum — 4 operation(s) for auxiliary methods.
- [Uzum Back-to-back Payment API](https://apis.io/apis/uzum/uzum-back-to-back-payment-api/) — The Back-to-back Payment API from Uzum — 1 operation(s) for back-to-back payment.
- [Uzum Check Service Status API](https://apis.io/apis/uzum/uzum-check-service-status-api/) — This method allows you to check the basic functionality and availability of the fiscalization service.
- [Uzum Common methods API](https://apis.io/apis/uzum/uzum-common-methods-api/) — The Common methods API from Uzum — 5 operation(s) for common methods.
- [Uzum Cross-border payments API](https://apis.io/apis/uzum/uzum-cross-border-payments-api/) — Methods for cross-border payments for services in Uzbekistan
- [Uzum Fiscalization API](https://apis.io/apis/uzum/uzum-fiscalization-api/) — In this section, the API provides methods for working with fiscal receipts. `/v2/receipt` method allows you to register a sale operation and generate a…
- [Uzum From Uzbekistan API](https://apis.io/apis/uzum/uzum-from-uzbekistan-api/) — Methods for transfers from Uzbekistan
- [Uzum Information retrieval API](https://apis.io/apis/uzum/uzum-information-retrieval-api/) — Auxiliary Information Methods
- [Uzum Managing Credit Cards Bindings API](https://apis.io/apis/uzum/uzum-managing-credit-cards-bindings-api/) — The Managing Credit Cards Bindings API from Uzum — 3 operation(s) for managing credit cards bindings.
- [Uzum Methods API](https://apis.io/apis/uzum/uzum-methods-api/) — QR CRM methods that are used to create an order, check the status of the order and transfer links to the fiscal receipt and cancel payment transactions.
- [Uzum One-Step Payment API](https://apis.io/apis/uzum/uzum-one-step-payment-api/) — The One-Step Payment API from Uzum — 2 operation(s) for one-step payment.
- [Uzum Partner API](https://apis.io/apis/uzum/uzum-partner-api/) — The partner API from Uzum — 2 operation(s) for partner.
- [Uzum Submit QR Code Payment Receipt to Tax Authorities API](https://apis.io/apis/uzum/uzum-submit-qr-code-payment-receipt-to-tax-authorities-api/) — The method is designed to send information about payments made using QR codes directly to the tax authority. Using this method allows automating the…
- [Uzum To Uzbekistan API](https://apis.io/apis/uzum/uzum-to-uzbekistan-api/) — Methods for transfers to Uzbekistan
- [Uzum Transfer process API](https://apis.io/apis/uzum/uzum-transfer-process-api/) — Methods used in the transfer process
- [Uzum Two-Step Payment API](https://apis.io/apis/uzum/uzum-two-step-payment-api/) — The Two-Step Payment API from Uzum — 2 operation(s) for two-step payment.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Uzum Merchant Webhooks](https://apis.io/asyncapis/uzum/uzum-merchant-webhooks/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [uzum-mcp.yml](https://apis.io/servers/uzum/uzum-mcp-yml/)

#### Overlays (9)

OpenAPI overlays, including conversational phrasing for each operation.

- [Uzum Checkout Overlay](https://apis.io/overlays/uzum/uzum-checkout-overlay/)
- [Uzum Crossborder Overlay](https://apis.io/overlays/uzum/uzum-crossborder-overlay/)
- [Uzum Dynamicqr Overlay](https://apis.io/overlays/uzum/uzum-dynamicqr-overlay/)
- [Uzum Fastpay Overlay](https://apis.io/overlays/uzum/uzum-fastpay-overlay/)
- [Uzum Fiscalization Overlay](https://apis.io/overlays/uzum/uzum-fiscalization-overlay/)
- [Uzum Merchant Overlay](https://apis.io/overlays/uzum/uzum-merchant-overlay/)
- [Uzum Nasiya Overlay](https://apis.io/overlays/uzum/uzum-nasiya-overlay/)
- [Uzum Ratekeeper Overlay](https://apis.io/overlays/uzum/uzum-ratekeeper-overlay/)
- [Uzum Remitcore Overlay](https://apis.io/overlays/uzum/uzum-remitcore-overlay/)

#### Security (2)

Authentication, trust centre, and vulnerability disclosure.

- [Uzum Authentication](https://apis.io/security/uzum/uzum-authentication/)
- [Uzum Domain Security](https://apis.io/security/uzum/uzum-domain-security/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Uzum Plans Pricing](https://apis.io/plans/uzum/uzum-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Uzum Rate Limits](https://apis.io/rate-limits/uzum/uzum-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://uzum.com/en/)
- [Documentation](https://developer.uzumbank.uz/en/)
- [Getting started](https://developer.uzumbank.uz/en/paymenthub/testing/getting-started/)
- [API reference](https://developer.uzumbank.uz/en/checkout)
- [Sign up](https://merchants.uzumbank.uz/en/)
- [Support](https://uzum.com/en/contacts/)
- [Blog](https://uzum.com/en/press-center/news-and-press-releases/)
- [Terms of service](https://uzum.com/en/privacy-and-terms/)
- [Privacy policy](https://uzum.com/en/privacy-and-terms/)

## API Evangelist rating

**Kin Score 46.3** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 71.4 |
| Contract quality | 53.1 |
| Contract governance | 4.5 |
| Operational transparency | 23.7 |
| Developer ergonomics | 57.7 |
| Access clarity | 42.1 |

**Agent readiness 38.9** — agent-ready (gated down from agent-native)

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | verified |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | verified |
| OpenAPI Examples | partial |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | yes |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | yes |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | na |
| Operation Distinctiveness | mostly (partial credit) |

**Regulatory alignment 39.3** — Payments

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/uzum/](https://apis.io/providers/uzum/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Uzum: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/uzum/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
