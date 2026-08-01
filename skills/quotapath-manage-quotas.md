---
name: Manage quota assignments and values
description: Assign quota targets to reps on a commission path and set quota values.
api: openapi/quotapath-openapi-original.json
operations: [path_quota_assignments_list, path_quota_assignments_create, path_quota_values_list, path_quota_values_create]
---

# Manage quota assignments and values

## Auth
`Authorization: Token <api-key>` (Premium tier).

## Steps
1. **List current assignments** — `GET /path/{path_uuid}/quota/assignments/`
   (`path_quota_assignments_list`) to see who is assigned on the path.
2. **Assign reps** — `POST /path/{path_uuid}/quota/assignments/`
   (`path_quota_assignments_create`) with assignment records (`email`,
   `start_date`, `end_date`).
3. **List quota values** — `GET /path/{path_uuid}/quota/values/`
   (`path_quota_values_list`).
4. **Set quota values** — `POST /path/{path_uuid}/quota/values/`
   (`path_quota_values_create`).

## Rules
- Resolve `path_uuid` from `plan_list` (a plan carries its `paths`).
- Pagination is limit/offset on both list endpoints.
