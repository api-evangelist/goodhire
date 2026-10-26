# GoodHire (goodhire)

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
