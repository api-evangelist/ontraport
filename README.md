# Ontraport

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
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Ontraport is a business automation platform combining CRM, marketing automation, email marketing, landing pages, ecommerce, and membership site capabilities for small businesses and entrepreneurs. The platform unifies contact management, sales pipelines, payment processing, and visual automation campaigns in a single workspace. Ontraport's REST API exposes contacts, transactions, tags, sequences, forms, landing pages, and ecommerce objects using API-Key and Api-Appid header authentication, through a generic object interface where the record type is selected by an integer object type ID rather than by a path segment. Ontraport also runs a first-party remote MCP server at https://mcp.ontraport.com, publishing 47 tools across CRUD, query, management and commerce and authorizing them with OAuth 2.1 (PKCE, dynamic client registration) under a single mcp:tools scope.

**Areas:** CRM · Marketing Automation · Email Marketing · E-Commerce · Landing Pages · Membership Sites · MCP · AI Agents · Payments · Sales Automation

## Ontraport on APIs.io

Everything profiled for Ontraport is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/ontraport/](https://apis.io/providers/ontraport/)** — or go straight to one of the APIs or properties below.

### APIs (4)

- [Ontraport REST API](https://apis.io/apis/ontraport/rest-api/) — RESTful API providing CRUD access to contacts, transactions, tags, sequences, forms, landing pages, and ecommerce objects. Authentication uses two headers,…
- [Ontraport Metadata API](https://apis.io/apis/ontraport/ontraport-metadata-api/) — Object metadata and field information.
- [Ontraport Objects API](https://apis.io/apis/ontraport/ontraport-objects-api/) — Generic CRUD operations across Ontraport object types.
- [Ontraport MCP Server](https://apis.io/apis/ontraport/mcp/) — Ontraport's first-party, hosted, remote MCP server. A streamable-HTTP endpoint an agent POSTs to directly at https://mcp.ontraport.com — there is no package…

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Ontraport Webhooks](https://apis.io/asyncapis/ontraport/ontraport-webhooks/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [Ontraport MCP Server](https://apis.io/servers/ontraport/ontraport-mcp-server/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Ontraport Agentic Access](https://apis.io/agentic-access/ontraport/ontraport-agentic-access/)

#### Collections (3)

Postman and Open Collection request collections.

- [Ontraport REST API](https://apis.io/collections/ontraport/open-ontraport/)
- [Ontraport REST Metadata API](https://apis.io/collections/ontraport/open-ontraport-metadata-api/)
- [Ontraport REST Metadata Objects API](https://apis.io/collections/ontraport/open-ontraport-objects-api/)

#### Overlays (2)

OpenAPI overlays, including conversational phrasing for each operation.

- [Ontraport Metadata Api Overlay](https://apis.io/overlays/ontraport/ontraport-metadata-api-overlay/)
- [Ontraport Objects Api Overlay](https://apis.io/overlays/ontraport/ontraport-objects-api-overlay/)

#### Security (4)

Authentication, trust centre, and vulnerability disclosure.

- [Ontraport Authentication](https://apis.io/security/ontraport/ontraport-authentication/)
- [Ontraport Domain Security](https://apis.io/security/ontraport/ontraport-domain-security/)
- [Ontraport Trust Center](https://apis.io/security/ontraport/ontraport-trust-center/)
- [Ontraport Vulnerability Disclosure](https://apis.io/security/ontraport/ontraport-vulnerability-disclosure/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Ontraport Scopes](https://apis.io/scopes/ontraport/ontraport-scopes/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Ontraport Plans Pricing](https://apis.io/plans/ontraport/ontraport-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Ontraport Rate Limits](https://apis.io/rate-limits/ontraport/ontraport-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://ontraport.com)
- [Documentation](https://api.ontraport.com/doc/)
- [Getting started](https://api.ontraport.com/doc/#authentication)
- [API reference](https://api.ontraport.com/doc/)
- [Pricing](https://ontraport.com/pricing)
- [Support](https://support.ontraport.com)
- [Blog](https://ontraport.com/blog/feed/)
- [Terms of service](https://ontraport.com/legal)
- [Privacy policy](https://ontraport.com/legal)
- [GitHub](https://github.com/Ontraport)
- [Status](https://ontraportstatus.com)

## API Evangelist rating

**Kin Score 64.2** — strong · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 75.0 |
| Contract quality | 56.1 |
| Contract governance | 18.2 |
| Operational transparency | 63.2 |
| Developer ergonomics | 67.3 |
| Access clarity | 85.5 |

**Agent readiness 54.9** — agent-native

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | — |
| MCP Server | verified |
| Auth Clarity | served (partial credit) |
| Idempotency | documented (partial credit) |
| Error Semantics | documented (partial credit) |
| OpenAPI Examples | — |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | yes |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | served (partial credit) |
| Protected Resource Metadata | verified |
| Dynamic Client Registration | yes |
| Agentic Commerce | — |
| Pagination Ratio | — |
| Operation Distinctiveness | distinct |

**Regulatory alignment 35.7** — Payments

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/ontraport/](https://apis.io/providers/ontraport/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Ontraport: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/ontraport/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
