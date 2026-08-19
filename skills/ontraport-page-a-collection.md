---
name: ontraport-page-a-collection
description: >-
  Read every record in an Ontraport collection without silently truncating at 50 and without
  exhausting the 180 requests/minute account budget. Use before any bulk read, export or
  segment walk.
api: ontraport:rest-api
generated: '2026-08-13'
method: generated
source: >-
  openapi/ontraport-objects-api-openapi.yml, openapi/ontraport-metadata-api-openapi.yml,
  https://api.ontraport.com/doc/#pagination, rate-limits/ontraport-rate-limits.yml
operations:
  - GET /objects/getInfo   # overlay operationId: getCollectionInfo
  - GET /objects           # overlay operationId: listObjects
  - GET /objects/meta      # overlay operationId: getObjectMeta
mcp_tools:
  - count_objects
  - get_objects
  - build_api_condition
---

# Page an Ontraport collection safely

The failure mode this skill exists to prevent: a collection read with no pagination returns
**exactly 50 records and an HTTP 200**. It does not error, it does not signal truncation. An
agent that reads a collection once and reports a total is reporting the first page.

## Steps

1. **Get the count before you read.** `GET /objects/getInfo` with `objectID=<type>` returns
   `data.count` for the collection, honouring any `condition` you pass. Since 2025-08-28 a
   `count` boolean parameter on the collection read can return the total alongside the
   results, which removes this call — prefer it when available.

2. **Decide whether to narrow instead of page.** Ontraport's own guidance is to use plural
   endpoints with `condition` and a field list rather than pulling everything. Narrowing the
   query is cheaper than paging it. `condition` takes a JSON array of `{field, op, value}`
   clauses:

   ```
   [{"field":{"field":"email"},"op":"=","value":{"value":"person@example.com"}}]
   ```

   On the MCP surface, `build_api_condition` produces exactly this JSON from a
   human-readable condition — build the condition with the agent, then execute it here.

3. **Page with `start` and `range`.** `GET /objects` with `objectID=<type>`, `range=50`
   (the maximum and the default) and `start` advancing by 50:

   ```
   GET https://api.ontraport.com/1/objects?objectID=0&start=0&range=50
   GET https://api.ontraport.com/1/objects?objectID=0&start=50&range=50
   GET https://api.ontraport.com/1/objects?objectID=0&start=100&range=50
   ```

   Stop when `start >= count` or a page returns fewer than `range` records.

4. **Ask for fewer fields.** Plural endpoints let you name exactly which fields to return.
   Singular endpoints (`GET /object`) always return everything — use them only for one
   known record.

5. **Budget the walk.** `ceil(count / 50)` requests, against 180 per minute shared with
   every other integration on the account. A 10,000-record collection is 200 calls — over a
   minute of the account's entire budget. Read `X-Rate-Limit-Remaining` on each response and
   pause when it gets low.

## Rate limiting

- Limit: 180 requests/minute per **account**, rolling.
- Headers on every response: `X-Rate-Limit-Limit`, `X-Rate-Limit-Remaining`,
  `X-Rate-Limit-Reset`.
- On `429`, wait `X-Rate-Limit-Reset` seconds. There is no `Retry-After`.

## Exceptions

- **Calendar events do not use `range`.** Page size is fixed by the `mode` parameter and
  paging is driven by moving the start timestamp. One mode does not paginate at all and
  returns events from one month back to six months forward.
- **`group_id`, not `group_ids`.** A collection can be limited by a single group at a time.
  `group_ids` was undocumented on 2020-01-09 but still functions.
- **Custom objects** use type IDs >= 10000 and vary per account. Resolve the ID from
  `GET /objects/meta` before paging.

## Rules

- Never report a total from an unpaginated read.
- Never page a collection from an agent loop. Ontraport explicitly directs bulk dataset
  work to the REST API or a workflow tool because the MCP server sees only a bounded set of
  records per call.
