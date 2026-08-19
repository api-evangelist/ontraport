---
name: ontraport-upsert-contact
description: >-
  Create or update an Ontraport contact without creating a duplicate, using the upsert
  Ontraport itself recommends. Use this instead of a plain create for any write where the
  record might already exist.
api: ontraport:rest-api
generated: '2026-08-13'
method: generated
source: >-
  openapi/ontraport-objects-api-openapi.yml, openapi/ontraport-metadata-api-openapi.yml,
  https://api.ontraport.com/doc/, conventions/ontraport-conventions.yml,
  errors/ontraport-problem-types.yml
operations:
  - POST /objects/saveorupdate    # overlay operationId: saveOrUpdateObject
  - GET /objects/meta             # overlay operationId: getObjectMeta
  - GET /object/getByEmail        # overlay operationId: getObjectByEmail
mcp_tools:
  - saveorupdate_object
  - get_object_meta
---

# Upsert an Ontraport contact

Ontraport has no `Idempotency-Key` header. The only repeat-safe write is the upsert:
`POST /objects/saveorupdate` matches on a unique field — normally `email` — and updates the
matched record instead of creating a second one. Ontraport names this "the recommended way
to add records because it prevents duplicates" on both the REST and MCP surfaces.

## Preconditions

- `Api-Key` and `Api-Appid` headers. Both are required; sending one without the other
  returns `401 Your App ID and API Key do not authenticate.`
- Base URL `https://api.ontraport.com/1`.
- You are inside a shared 180 requests/minute rolling account budget.

## Steps

1. **Discover the fields first.** Custom fields vary per account, so no static schema
   exists. Call `GET /objects/meta` with `objectID=0` (Contact) and read the field list.
   Skip this only if you have already cached the metadata for this account.

2. **Write with the upsert.** `POST /objects/saveorupdate` with `objectID=0` and a body
   carrying the unique field plus the fields you want set:

   ```
   POST https://api.ontraport.com/1/objects/saveorupdate
   Api-Key: <key>
   Api-Appid: <app-id>
   Content-Type: application/json

   {"objectID": 0, "email": "person@example.com", "firstname": "...", "lastname": "..."}
   ```

   If you know the record's `unique_id`, send it too — it tells Ontraport exactly which
   object to update. `unique_id` is not editable and exists on ORM objects since 2018-06-27.

3. **Read the envelope, not just the status.** Every response is
   `{code, data, account_id}`; `code: 0` means the HTTP 200 was a real success. Since
   2017-07-25, `/saveorupdate` returns all updated fields and the object's ID.

4. **Verify only if you must.** `GET /object/getByEmail` resolves an email to an object ID.
   Use it for reconciliation, not on the happy path — it costs a call against the same
   rate-limit budget.

## Error handling

| Status | Meaning | What to do |
| --- | --- | --- |
| 400 | Malformed request data | Do not retry. Recheck `objectID` and field names against `/objects/meta`. |
| 401 | Missing or mismatched credentials | Do not retry. Both headers must be present and from the same account. |
| 403 | Permission denied | Do not retry. Package-level and user-level permissions have applied to API requests since 2019-02-01 — check the permissions of the user who owns the key. |
| 422 | Invalid email address | Do not retry. Validate the address; this status exists specifically for it (added 2019-10-22). |
| 429 | Rate limit exceeded | Retry after `X-Rate-Limit-Reset` seconds. |
| 500 | Server error | Retry with backoff; check https://ontraport.com/service-status. |

## Rules

- **Never** retry a plain `POST /objects` after a timeout. It has no idempotency and will
  create a second record. Convert the write to `/objects/saveorupdate` first.
- The upsert is idempotent **per unique-field match**, not per request. Two different
  payloads matching the same email will both apply; the second overwrites the first.
- Watch `X-Rate-Limit-Remaining` on every response and stop before it reaches zero.
