# mailboxlayer

mailboxlayer is an [apilayer](https://apilayer.com)-owned REST JSON API for **email address verification**. It combines syntax checks, MX-record lookups, real-time SMTP verification, catch-all detection, role-address detection, free / disposable provider detection, did-you-mean typo suggestions, and a numeric deliverability quality score in one call.

- **Base URL:** `https://apilayer.net/api`
- **Marketplace:** [apilayer.com/marketplace/mailboxlayer-api](https://apilayer.com/marketplace/mailboxlayer-api)
- **Documentation:** [docs.apilayer.com/mailboxlayer](https://docs.apilayer.com/mailboxlayer/docs/api-documentation)
- **Pricing:** [mailboxlayer.com/product](https://mailboxlayer.com/product)
- **Type:** company (x-type)

**APIs.yml:** [apis.yml](apis.yml)

## API

### mailboxlayer Verification API

REST JSON API for verifying a single email address.

- **Endpoint:** `GET /api/check?email={email}&access_key={key}`
- **Auth:** `access_key` query parameter
- **Response fields:** `email`, `did_you_mean`, `user`, `domain`, `format_valid`, `mx_found`, `smtp_check`, `catch_all`, `role`, `disposable`, `free`, `score`

#### Artifacts
- **OpenAPI:** [openapi/mailboxlayer-openapi.yml](openapi/mailboxlayer-openapi.yml)
- **JSON Schema:** [check result](json-schema/mailboxlayer-check-result-schema.json), [error envelope](json-schema/mailboxlayer-error-schema.json)
- **JSON Structure:** [check result](json-structure/mailboxlayer-check-result-structure.json)
- **JSON-LD Context:** [mailboxlayer-context.jsonld](json-ld/mailboxlayer-context.jsonld)
- **Examples:** [success](examples/mailboxlayer-check-success-example.json), [typo / did-you-mean](examples/mailboxlayer-check-typo-example.json), [disposable](examples/mailboxlayer-check-disposable-example.json), [error](examples/mailboxlayer-check-error-example.json), [result](examples/mailboxlayer-check-result-example.json), [free-provider result](examples/mailboxlayer-check-result-free-example.json)
- **Naftiko Capability:** [mailboxlayer-verification.yaml](capabilities/mailboxlayer-verification.yaml)

## Cross-Cutting Artifacts

- **Spectral Rules:** [rules/mailboxlayer-rules.yml](rules/mailboxlayer-rules.yml)
- **Vocabulary:** [vocabulary/mailboxlayer-vocabulary.yml](vocabulary/mailboxlayer-vocabulary.yml)
- **Plans & Pricing:** [plans/mailboxlayer-plans-pricing.yml](plans/mailboxlayer-plans-pricing.yml)
- **Rate Limits:** [rate-limits/mailboxlayer-rate-limits.yml](rate-limits/mailboxlayer-rate-limits.yml)
- **FinOps:** [finops/mailboxlayer-finops.yml](finops/mailboxlayer-finops.yml)

## Features

- Syntax and typo check (with did-you-mean suggestion)
- Real-time SMTP verification
- MX record lookup
- Catch-all domain detection (paid plans)
- Role address detection (`support@`, `admin@`, etc.)
- Free email provider detection (Gmail, Yahoo, Outlook.com)
- Disposable email provider detection (mailinator.com, etc.)
- 0.0 to 1.0 deliverability quality score
- 256-bit HTTPS encryption (paid plans)
- Bulk endpoint (25 / 100 emails per request on Pro Plus / Enterprise Plus)
- JSONP support via `callback` parameter

## Plans

| Plan              | Price (monthly) | Requests   | HTTPS | Bulk     | Support |
|-------------------|-----------------|------------|-------|----------|---------|
| Free              | $0              | 100        | No    | -        | None    |
| Basic             | $14.99          | 5,000      | Yes   | -        | Standard |
| Professional Plus | $74.99          | 50,000     | Yes   | 25 / req | Standard |
| Enterprise Plus   | $249.99         | 250,000    | Yes   | 100 / req | Standard |
| Custom            | Contact sales   | Negotiated | Yes   | -        | Standard |

Annual billing discounts: Basic 10%, Pro Plus 12.5%, Enterprise Plus 15%. Optional **Platinum Support** annual add-on (dedicated AM, priority support, quarterly briefings).

## Use Cases

- Signup form validation
- Email list hygiene
- Lead enrichment
- Fraud and abuse prevention (disposable email blocking)
- Transactional email hygiene
- B2B sales qualification

## Integrations & SDKs

- **PostHog** — [mailboxlayer-plugin](https://github.com/PostHog/mailboxlayer-plugin)
- **Fortinet FortiSOAR** — [connector-mailboxlayer](https://github.com/fortinet-fortisoar/connector-mailboxlayer)
- **Laravel (PHP)** — [ash-jc-allen/laravel-mailboxlayer](https://github.com/ash-jc-allen/laravel-mailboxlayer)
- **Symfony** — [ylly/mailboxlayerbundle](https://github.com/ylly/mailboxlayerbundle)
- **Node.js / TypeScript** — [damienmarchandfr/mailboxlayer-node-client](https://github.com/damienmarchandfr/mailboxlayer-node-client)
- **Ruby** — [actfong/mailboxlayer](https://github.com/actfong/mailboxlayer)

## Error Codes

| Code | Type | Meaning |
|------|------|---------|
| 101 | `invalid_access_key` | Bad or missing API access key |
| 103 | `invalid_api_function` | Endpoint does not exist |
| 104 | `usage_limit_reached` | Monthly quota exhausted |
| 105 | `https_access_restricted` | HTTPS not enabled on current plan |
| 106 | `inactive_user` | Account inactive or suspended |
| 210 | `no_email_address_supplied` | `email` parameter missing |
| 211 | `invalid_email_address` | `email` parameter syntactically invalid |

## Tags

Email, Email Verification, Email Validation, SMTP, MX Records, Catch-All Detection, Disposable Email, Free Email Provider, Role Address, Quality Score, apilayer, Public APIs

## Source / Provenance

- Bulk-registered from [public-apis/public-apis](https://github.com/public-apis/public-apis) on 2026-05-28 (category: Email).
- Enriched via the api-evangelist `run-pipeline` skill (company / 18-step) on 2026-05-30.

## Timestamps

- **Created:** 2026-05-28
- **Modified:** 2026-05-30

## Maintainers

- **Kin Lane** — kin@apievangelist.com
