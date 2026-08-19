---
name: Clean an email list in bulk with mailboxlayer
description: >-
  Verify many addresses efficiently using the plan-gated /bulk_check endpoint — batch
  sizing, the array-vs-object response ambiguity, quota arithmetic, and the fallback to
  per-address verification when the plan does not include bulk.
api: openapi/_original/mailboxlayer-swaggerhub-openapi.json
operations: [bulkCheckEmails, checkEmail]
generated: '2026-08-14'
method: generated
source: >-
  openapi/_original/mailboxlayer-swaggerhub-openapi.json,
  https://docs.apilayer.com/mailboxlayer/docs/getting-started
---

# Clean an email list in bulk

Use this to run list hygiene over an existing marketing list, CRM export, or signup backlog.

## Check the plan first — bulk is gated

`/bulk_check` is **not available on Free or Basic**. Calling it there returns error `105`
(`function_access_restricted`, HTTP 403).

| Plan | Bulk access | Max addresses per call |
|---|---|---|
| Free | no | — |
| Basic | no | — |
| Professional Plus | yes | 25 |
| Enterprise Plus | yes | 100 |

If bulk is unavailable, fall back to `checkEmail` (`GET /check`) one address at a time —
see `skills/mailboxlayer-verify-single-address.md`. Budget accordingly: 10,000 addresses is
10,000 billable requests on Basic, but only 400 calls (at 25/call) on Professional Plus.

## Call `bulkCheckEmails`

`GET /bulk_check`

| Parameter | Required | Notes |
|---|---|---|
| `access_key` | yes | Account access key. |
| `emails` | yes | **Comma-separated** address list in the query string. |
| `smtp` | no | `1` (default). Set `0` to skip SMTP — much faster over a large list. |
| `catch_all` | no | `0` (default). `1` is materially slower; think before enabling it across a whole list. |
| `format` | no | Leave at `0`. Pretty-printing a 100-result array wastes bandwidth. |
| `callback` | no | JSONP wrapper. |

```
GET https://apilayer.net/api/bulk_check?access_key=YOUR_ACCESS_KEY&emails=user1@domain.com,user2@gmail.com
```

Note the parameter is `emails` (plural). The provider's own published code examples pass
singular `email` to `/bulk_check`; that is a defect in their examples, not a supported form.

## Read the response — check the JSON type before indexing

`/bulk_check` returns a `oneOf`: either an **array** of result objects, one per submitted
address, **or** a single **object** carrying the error envelope. There is no wrapper telling
you which. Type-check the top-level value first:

```
if (Array.isArray(body))      -> results, one per address, in submission order
else if (body.success === false) -> whole-call failure, read body.error.type
```

Indexing the response as an array without that check will throw on every error path.

Specific failures worth handling by name:

- `bulk_limit_exceeded` (code `231`) — you sent more addresses than the plan allows in one
  call. Chunk to 25 or 100 and retry.
- `function_access_restricted` (code `105`) — the plan has no bulk access at all.
- `usage_limit_reached` / `daily_usage_limit_reached` / `fair_use_limit_reached` (all code
  `104`) — monthly, daily, or an undisclosed fair-use ceiling. Only the monthly figure is
  published; the other two are discoverable only by hitting them.

Each element of a successful array is the same shape `checkEmail` returns, and carries the
same traps — `catch_all` is tri-state (`null` ≠ `false`), `did_you_mean` is advisory only,
`score` has no published bands.

## Quota arithmetic

**One bulk call counts as ONE billable request** regardless of how many addresses it
carries. That makes bulk the cheapest path against both the monthly allowance and the
per-minute rate limit:

- Professional Plus: 50,000 requests/month × 25 = up to 1.25M addresses.
- Enterprise Plus: 250,000 requests/month × 100 = up to 25M addresses.

Per-minute ceilings still apply to the number of *calls*, not addresses: Professional 300,
Enterprise 300, and the provider recommends staying under 5 requests/second. At 100
addresses per call and 5 calls/second, that is 500 addresses/second.

## Running a whole list

1. Deduplicate locally first — you pay per request, and mailboxlayer stores nothing, so a
   repeated address is a repeated charge.
2. Chunk to the plan ceiling (25 or 100), preserving order so you can map results back.
3. Set `smtp=1` for hygiene work — skipping SMTP removes the single most valuable signal.
   Leave `catch_all=0` unless catch-all domains actually matter to your decision.
4. Pace with a client-side token bucket. **There are no rate-limit headers and no
   `Retry-After`**, so nothing tells you how much budget is left or how long to wait.
5. Retry failed chunks freely — `GET` is safe and idempotent, and there is no idempotency
   key because there is nothing to de-duplicate.
6. Persist your own results. There is no id, no handle, and no way to re-read a prior
   verification: mailboxlayer is stateless and returns nothing you can look up later.

## References

- `conventions/mailboxlayer-conventions.yml` — batching, error envelopes, rate-limit signalling
- `errors/mailboxlayer-problem-types.yml` — every code, type and status
- `plans/mailboxlayer-plans-pricing.yml` — tiers, allowances, overage rates
- `data-model/mailboxlayer-data-model.yml` — the array-vs-object response union
