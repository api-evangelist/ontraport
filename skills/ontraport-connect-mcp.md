---
name: ontraport-connect-mcp
description: >-
  Connect an MCP client to Ontraport's first-party remote MCP server, choose the right
  authentication path, and set the tool permissions before letting an agent touch a live CRM
  and payment surface. Use whenever an agent needs Ontraport access.
api: ontraport:mcp
generated: '2026-08-13'
method: generated
source: >-
  https://ontraport.com/support/My-account/mcp-server,
  well-known/ontraport-oauth-protected-resource.json,
  well-known/ontraport-oauth-authorization-server.json, mcp/ontraport-mcp.yml,
  scopes/ontraport-scopes.yml
operations:
  - POST https://mcp.ontraport.com   # MCP JSON-RPC endpoint
mcp_tools:
  - list_allowed_object_types
  - get_object_meta
  - get_account_info
---

# Connect an agent to Ontraport MCP

Ontraport runs a hosted, remote MCP server at `https://mcp.ontraport.com`. It is a
streamable-HTTP endpoint — there is nothing to install and no stdio process. The endpoint is
the bare host root; `POST /mcp` returns 404.

## Choose an auth path

**OAuth (default).** Ontraport publishes complete discovery metadata:

- `https://mcp.ontraport.com/.well-known/oauth-protected-resource` → resource,
  `authorization_servers: ["https://app.ontraport.com"]`, `scopes_supported: ["mcp:tools"]`
- `https://app.ontraport.com/.well-known/oauth-authorization-server` → authorization code +
  refresh token, PKCE `S256` required, dynamic client registration at `/oauth/register`,
  revocation at `/oauth/revoke`

An unauthenticated `tools/list` returns `401` with
`WWW-Authenticate: Bearer resource_metadata="…/.well-known/oauth-protected-resource",
scope="mcp:tools"` — a correct RFC 9728 challenge, so a compliant client can discover the
whole flow from the error.

```bash
claude mcp add --scope user --transport http ontraport https://mcp.ontraport.com
```

**API key headers (fallback).** For orchestration platforms and agent frameworks that
cannot run OAuth. The App ID and API Key are NOT an OAuth client id/secret — they go in
headers:

```bash
claude mcp add --scope user --transport http ontraport https://mcp.ontraport.com \
  --header "Api-Appid: <app-id>" --header "Api-Key: <api-key>"
```

An `Authorization: Bearer <app-id>:<api-key>` single-header form is also accepted. Cursor
takes a plain `{"mcpServers": {"ontraport": {"url": "https://mcp.ontraport.com"}}}` entry.

## Set permissions before the first real prompt

There is **one scope, `mcp:tools`, and it grants all 47 tools** — including
`process_transaction`, `refund_transaction`, `cancel_subscription` and `delete_objects`.
The authorization layer cannot give an agent read-only access. The only control is
client-side: after connecting, use the configure control on the connection to enable or
disable individual tools.

- For an analysis or reporting agent: keep **Query** enabled, disable Commerce and the
  delete tools. The Query tools are what the agent uses for structural discovery — turning
  them off breaks everything else.
- For a data-entry agent: keep CRUD and Manage, disable Commerce.
- Only enable Commerce for an agent whose whole job is billing, and only with a human in the
  loop on the charge and refund tools.

Ontraport's server-side guardrails are narrow: it ignores requests to delete every record in
a collection, and it disables generic writes on Invoices, Payments and Orders so those must
go through the specialized tools. Everything else is on you.

## First three calls

1. `get_account_info` — confirm you are pointed at the account you think you are.
2. `list_allowed_object_types` — the record types reachable through MCP. This is not the
   same as the account's in-app permissions; not every object is exposed.
3. `get_object_meta` on each type you will write to — custom fields vary per account and
   there is no static schema.

## Know the limits before you promise anything

- **Bounded working set.** The agent sees a fraction of the account's records per call. Do
  not ask it to "update every contact who…" — it will answer confidently about a subset.
  Ontraport itself directs full-dataset work to the REST API or a workflow tool.
- **Shared rate limit.** MCP calls consume the same 180 requests/minute account budget as
  the REST API. An agent loop can starve a production integration.
- **`build_api_condition` is the handoff.** Use the agent to define a segment, have it emit
  the raw API condition, then execute the bulk action through the REST API.
- **Prefer `saveorupdate_object`** over `create_object` for any write that might duplicate.
  It is the only repeat-safe write on the platform.
