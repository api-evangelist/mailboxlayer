# mailboxlayer (mailboxlayer)

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
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

mailboxlayer is an apilayer-owned REST JSON API for email address verification. It performs syntax and typo checks, MX-record lookup, real-time SMTP verification, catch-all detection, role-address detection, free and disposable provider detection, and returns a numeric deliverability quality score. Useful for signup-form validation, list hygiene, lead enrichment, and fraud prevention.

**APIs.json:** [https://mailboxlayer.com](https://mailboxlayer.com)

## Tags

- Email
- Email Verification
- Email Validation
- SMTP
- MX Records
- Catch-All Detection
- Disposable Email
- Free Email Provider
- Role Address
- Quality Score
- apilayer
- Public APIs

## Timestamps

- **Created:** 2026-05-28
- **Modified:** 2026-05-30

## APIs

### mailboxlayer Verification API

REST JSON API for verifying a single email address. Performs syntax validation, MX-record lookup, real-time SMTP check, catch-all detection, role-address detection, free / disposable provider detection, did-you-mean suggestion, and a 0.0 - 1.0 deliverability quality score.

- **Human URL:** [https://mailboxlayer.com](https://mailboxlayer.com)
- **Base URL:** `https://apilayer.net/api`

#### Tags

- Email Verification
- SMTP
- MX Records
- Quality Score

#### Properties

- [Documentation](https://docs.apilayer.com/mailboxlayer/docs/api-documentation)
- [API Reference](https://docs.apilayer.com/mailboxlayer/docs/api-documentation)
- [Quickstart](https://mailboxlayer.com/quickstart)
- [Authentication](https://docs.apilayer.com/mailboxlayer/docs/api-documentation)
- [OpenAPI](openapi/mailboxlayer-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/mailboxlayer.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/mailboxlayer.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [JSON Schema](json-schema/mailboxlayer-check-result-schema.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON Schema](json-schema/mailboxlayer-error-schema.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON Structure](json-structure/mailboxlayer-check-result-structure.json)
- [J S O N- L D](json-ld/mailboxlayer-context.jsonld)
- [Example](examples/mailboxlayer-check-success-example.json)
- [Example](examples/mailboxlayer-check-typo-example.json)
- [Example](examples/mailboxlayer-check-disposable-example.json)
- [Example](examples/mailboxlayer-check-error-example.json)
- [Example](examples/mailboxlayer-check-result-example.json)
- [Example](examples/mailboxlayer-check-result-free-example.json)
- [Rate Limits](rate-limits/mailboxlayer-rate-limits.yml)

## Common Properties

- [Website](https://mailboxlayer.com)
- [Portal](https://mailboxlayer.com/dashboard)
- [Sign Up](https://mailboxlayer.com/product)
- [Pricing](https://mailboxlayer.com/product)
- [Getting Started](https://mailboxlayer.com/quickstart)
- [Terms of Service](https://mailboxlayer.com/terms)
- [Privacy Policy](https://mailboxlayer.com/privacy)
- [Support](https://mailboxlayer.com/support)
- [GitHub Organization](https://github.com/apilayer)
- [GitHub Repository](https://github.com/apilayer/mailboxlayer-API)
- [SDK](https://github.com/ash-jc-allen/laravel-mailboxlayer)
- [SDK](https://github.com/actfong/mailboxlayer)
- [SDK](https://github.com/damienmarchandfr/mailboxlayer-node-client)
- [SDK](https://github.com/ylly/mailboxlayerbundle)
- [Integrations](https://github.com/PostHog/mailboxlayer-plugin)
- [Integrations](https://github.com/fortinet-fortisoar/connector-mailboxlayer)
- [Spectral Rules](rules/mailboxlayer-rules.yml)
- [Vocabulary](vocabulary/mailboxlayer-vocabulary.yml)
- [Public APIs Listing](https://github.com/public-apis/public-apis)
- [Plans](plans/mailboxlayer-plans-pricing.yml)
- [Rate Limits](rate-limits/mailboxlayer-rate-limits.yml)
- [Fin Ops](finops/mailboxlayer-finops.yml)
- [Features](undefined)
- [Use Cases](undefined)
- [Integrations](undefined)
- [Solutions](undefined)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
