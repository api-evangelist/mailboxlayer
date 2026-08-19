---
name: Verify a single email address with mailboxlayer
description: >-
  Validate one email address end to end — syntax, MX, live SMTP, catch-all, role,
  disposable and free-provider detection, plus a 0.0-1.0 deliverability score — and read
  the result correctly, including the failure modes mailboxlayer hides behind HTTP 200.
api: openapi/_original/mailboxlayer-swaggerhub-openapi.json
operations: [checkEmail]
generated: '2026-08-14'
method: generated
source: >-
  openapi/_original/mailboxlayer-swaggerhub-openapi.json,
  https://docs.apilayer.com/mailboxlayer/docs/getting-started
---

# Verify a single email address

Use this when you have one address and need to know whether mail sent to it will land.

## Before you call

- Base URL: `https://apilayer.net/api`
- Auth: an APILayer access key passed as the **`access_key` query parameter**. There is no
  header form. The key ends up in logs, browser history and request traces — **redact
  `access_key` before writing any transcript**.
- One key covers every APILayer product on the account, so treat it as an account-wide
  credential, not a mailboxlayer-scoped one.

## Call `checkEmail`

`GET /check`

| Parameter | Required | Notes |
|---|---|---|
| `access_key` | yes | Account access key. |
| `email` | yes | The address to verify. |
| `smtp` | no | `1` (default) runs the live SMTP conversation. `0` skips it — faster, but `smtp_check` becomes meaningless. |
| `catch_all` | no | `0` (default). Send `1` to run catch-all detection. Paid plans only. |
| `format` | no | `1` pretty-prints. Debugging only; it inflates the payload. |
| `callback` | no | JSONP wrapper. Leave unset for server-side calls. |

```
GET https://apilayer.net/api/check?access_key=YOUR_ACCESS_KEY&email=support@apilayer.com
```

## Read the response — this is the part that goes wrong

**A 200 is not evidence the verification ran.** mailboxlayer returns service-level failures
with HTTP 200 and a `success: false` body. Branch in this order:

1. **Is the body an object with `success === false`?** Then it is an error, whatever the
   status code says. Read `error.type` — not `error.code`, which is not unique (`101` is
   both missing and invalid key; `104` is monthly, daily, fair-use exhaustion *and* a
   blocked account; `105` is both HTTPS and function restriction).
2. **Is the status 422?** Then the body is a *different* shape — `{"detail":[{"loc":...,
   "msg":...,"type":...}]}` — with no `success` flag at all. A required query parameter was
   missing or malformed.
3. **Is the status 401 / 403 / 404 / 429 / 500 / 503?** The body is the normal error
   envelope. See `errors/mailboxlayer-problem-types.yml` for the full catalog.
4. Otherwise you have a result. Read the fields below.

### Result fields

| Field | Meaning |
|---|---|
| `format_valid` | Syntax is valid per RFC 5322/5321. |
| `mx_found` | The domain has MX records. |
| `smtp_check` | The mail server accepted the mailbox in a live SMTP conversation. |
| `catch_all` | **Tri-state.** `true` / `false` / `null`. |
| `role` | Function address (`support@`, `postmaster@`), not a person. |
| `disposable` | Throwaway provider. |
| `free` | Free webmail provider. |
| `score` | 0.0-1.0 composite deliverability score. |
| `did_you_mean` | Typo suggestion, or empty string. |

**Three traps:**

- `catch_all: null` does **not** mean false. It means the check did not run — either you
  did not send `catch_all=1`, or the plan does not permit it (error `310`,
  `catch_all_access_restricted`). Treating null as false silently misclassifies catch-all
  domains as verified.
- `did_you_mean` is **advisory only**. Every other field describes the address *as
  submitted*, not the suggestion. If you act on the suggestion, re-verify it.
- `score` has **no published bands**. The provider's quickstart calls 0.8 "generally
  considered high-quality and deliverable"; that is the only guidance given. Any cutoff you
  pick is your policy, not theirs — say so wherever you report on it.

## Pacing

Published per-minute ceilings, by plan: Free 50, Basic 100, Professional 300,
Enterprise 300. The provider recommends staying under **5 requests/second**.

There are **no rate-limit response headers** — no `X-RateLimit-*`, no `RateLimit-*`, no
`Retry-After`. You cannot read remaining budget and you are not told how long to wait. Run a
client-side token bucket set to your plan's per-minute figure, and back off exponentially on
`error.type` of `rate_limit_reached` (106) or any `104`.

`GET /check` is safe and idempotent, so **retrying is free and correct**. There is no
idempotency key because there is nothing to de-duplicate.

## References

- `conventions/mailboxlayer-conventions.yml` — full request/response semantics
- `errors/mailboxlayer-problem-types.yml` — every code, type and status
- `rate-limits/mailboxlayer-rate-limits.yml` — limits, quotas, undisclosed ceilings
- `authentication/mailboxlayer-authentication.yml` — credential handling and hazards
