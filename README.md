# GoodHire (goodhire)

GoodHire is an FCRA-compliant employment background check platform aimed at small and midsize businesses, offering criminal, identity, employment, education, and reference screening with candidate self-consent workflows. GoodHire was acquired by **Checkr** (the developer-first background check company) in **2021** - the deal, part of Checkr's acquisition of GoodHire's parent Inflection, was reported at roughly **$400M** and was Checkr's largest to date - and now operates as "GoodHire, A Checkr Company." GoodHire continues under its own brand and API, focused on the SMB segment, while Checkr targets larger and developer-first customers.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/goodhire/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/goodhire/refs/heads/main/apis.yml)

## Access Model

GoodHire exposes a **real, documented RESTful API**, but it is **gated**, not fully self-service:

- **Documentation is public** at [goodhire.com/api](https://www.goodhire.com/api/) and the help center docs.
- **API keys are issued on request** - developers contact **api@goodhire.com** to obtain a key.
- **Two API flavors:** a **Customer API** (a single company ordering its own reports) and a **Partner API** (HR platforms that let their employer customers order GoodHire reports through an embedded flow).
- **Base URLs:** `https://api.goodhire.com` (production) and `https://api-sandbox.goodhire.com` (sandbox, which returns dummy report data for building integrations).
- **Authentication:** API key sent in the `Authorization` header as `ApiKey <API_KEY>`.
- **Real-time updates:** delivered via outbound **HTTP webhooks** on report status changes - there is **no public WebSocket API**.

Background checks are ordered by creating a **report** object tied to a **candidate** and a **screening package (product bundle)**; every report is associated with a **requestor** (the user running it).

> **Modeling note:** Some endpoint paths and schemas in this catalog are **confirmed** from GoodHire's public docs (report queue/status, product bundles, partner app access). Others are **honestly modeled** (`x-endpointsModeled`) from documented resource patterns because the full reference could not be crawled directly at build time. Treat the OpenAPI as a discovery aid, not a byte-for-byte mirror.

## Tags

- Background Checks
- Employment Screening
- Identity Verification
- HR
- Compliance
- FCRA
- Checkr

## Timestamps

- **Created:** 2026-07-03
- **Modified:** 2026-07-03

## APIs

### GoodHire Reports API

Order and track background check reports. A report is created against a candidate and a screening package, then queued for processing; status and results are polled or received via webhook. Confirmed operations: queue a report and get a report by ID.

- **Human URL:** [https://www.goodhire.com/api/](https://www.goodhire.com/api/)
- **Base URL:** `https://api.goodhire.com`
- Confirmed: `POST /company/{company_id}/requestor/{requestor_id}/report/queue`, `GET /company/{company_id}/requestor/{requestor_id}/report/{report_id}`

### GoodHire Requestors API

Manage requestor objects - the users within a company who order and are associated with reports. Requestor provisioning is a prerequisite for ordering reports, especially in the Partner API.

- **Human URL:** [https://help.goodhire.com/docs/partner-requestors](https://help.goodhire.com/docs/partner-requestors)
- **Base URL:** `https://api.goodhire.com`

### GoodHire Packages API

Retrieve the screening packages (product bundles) available to an account along with their prices, so an integration can present the right package and per-check cost before ordering. Confirmed: `GET /company/{company_id}/requestor/{requestor_id}/productbundles`.

- **Human URL:** [https://www.goodhire.com/pricing/](https://www.goodhire.com/pricing/)
- **Base URL:** `https://api.goodhire.com`

### GoodHire Webhooks API

Receive real-time status updates as a report moves through its lifecycle (queued, pending candidate consent, in progress, complete, action required). Outbound HTTP callbacks, not a WebSocket.

- **Human URL:** [https://www.goodhire.com/api/](https://www.goodhire.com/api/)
- **Base URL:** `https://api.goodhire.com`

### GoodHire Partner API

The Partner API surface for HR platforms that let their employer customers order GoodHire reports, including the partner app-access flow used to select and configure a report for an embedded requestor. Confirmed: `POST /company/{company_id}/requestor/{requestor_id}/partnerappaccess/stepselectreport`.

- **Human URL:** [https://help.goodhire.com/docs/partner-reports](https://help.goodhire.com/docs/partner-reports)
- **Base URL:** `https://api.goodhire.com`

## Pricing (per completed check)

GoodHire bills per report by package, not per API call. Public preset tiers (employers running 50+ checks/year; subject to change):

- **Basic** - $29.99: SSN Trace, Sex Offender Registry, Global Watchlist, National Criminal Search
- **Essential** - $59.99: adds Identity Verification and Unlimited County Criminal Search
- **Complete** - $94.99: adds Unlimited State Criminal Search and Federal Criminal Search
- **Custom / Add-Ons** - 100+ add-on screenings (employment/education verification, references, MVR, drug testing, international); custom packages for employers under 50 checks/year.

## Common Properties

- [LinkedIn](https://www.linkedin.com/company/goodhire)
- [Website](https://www.goodhire.com/)
- [Documentation](https://www.goodhire.com/api/)
- [Pricing](https://www.goodhire.com/pricing/)
- [Plans](plans/goodhire-plans-pricing.yml)
- [Rate Limits](rate-limits/goodhire-rate-limits.yml)
- [Fin Ops](finops/goodhire-finops.yml)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
