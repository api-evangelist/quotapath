---
name: Push a deal and read its payout
description: Create a deal in QuotaPath, then read the resulting commission payout for the assigned rep.
api: openapi/quotapath-openapi-original.json
operations: [deal_create, deal, payout_resolved_list, payout_unresolved_list]
---

# Push a deal and read its payout

Use the QuotaPath REST API (`https://api.quotapath.com/v1`) to record a deal and
inspect the commission it generates.

## Auth
Send `Authorization: Token <api-key>` on every request. API access requires the
Premium tier.

## Steps
1. **Create the deal** — `POST /deal/` (`deal_create`). Provide the deal `name`,
   `date`, `user_email` of the owning rep, an `integration_id` + `integration_source`
   for idempotent external de-duplication, and `deal_values` (each with a `value`
   and `path_id`).
2. **Confirm it** — `GET /deal/{uuid}/` (`deal`) using the `id` returned in step 1.
3. **Read payouts** — poll `GET /payout/unresolved/` (`payout_unresolved_list`) for
   commissions still pending, and `GET /payout/resolved/` (`payout_resolved_list`)
   once approved. Filter incrementally with `updated_since`. Match rows by
   `deal_uuid` and `target_user_email`.

## Rules
- Pagination is limit/offset; follow the `next` URI until null.
- There is no idempotency-key header — set `integration_id`/`integration_source`
  so re-pushing the same source deal upserts rather than duplicates.
