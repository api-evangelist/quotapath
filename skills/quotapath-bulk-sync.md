---
name: Bulk-sync deals from an external system
description: Push many deals (or arbitrary external records) into QuotaPath in one call from a CRM or warehouse job.
api: openapi/quotapath-openapi-original.json
operations: [deal_bulk_create, data_bulk-import_create, deal_list]
---

# Bulk-sync deals from an external system

## Auth
`Authorization: Token <api-key>` (Premium tier).

## Steps
1. **Bulk-create deals** — `POST /deal/bulk/` (`deal_bulk_create`) with a
   `deals` array (each item an ExternalBulkDeal: `name`, `date`, `user_email`,
   `integration_id`, `integration_source`, `deal_values`, optional `metadata`).
2. **Or upsert generic records** — `POST /data/bulk-import/` (`data_bulk-import_create`)
   with `data_source`, `data_type`, `data_id_field`, and the `data` payload when
   syncing records that are not native deals.
3. **Verify** — `GET /deal/` (`deal_list`) filtered by `integration_id`,
   `path_id`, or `user_email` to confirm the sync landed.

## Rules
- Use a stable `integration_id` per source record so repeated syncs upsert.
- Follow limit/offset pagination on the verify list call.
