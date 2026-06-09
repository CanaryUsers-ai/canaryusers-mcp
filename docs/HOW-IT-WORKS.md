# How the CanaryUsers MCP server works

A short, accurate description of the public behavior of the remote server. (No
internal implementation details — this repo is public.)

## Transport

- **Remote, Streamable HTTP.** One endpoint: `POST https://www.canaryusers.ai/api/mcp`.
- JSON‑RPC 2.0 over POST, `application/json` request and response. Single messages
  or batches are accepted. It's a **tools‑only** server (no resources/prompts), so
  no SSE stream is needed for synchronous calls.
- A `GET` to the endpoint returns `405` with `Allow: POST` — that's expected; connect
  with an MCP client, not a browser.

## Authentication

Two layers, by design:

1. **Discovery handshake is open (no token).** The read‑only methods
   `initialize`, `notifications/initialized`, `notifications/cancelled`,
   `tools/list`, and `ping` are answered **without** a token. This lets registries
   (Smithery, Glama) and clients introspect the server and show its tools.
   `tools/list` only returns the public tool *schemas* — the same ones in
   `server.json` and the registry — so nothing account‑bound is exposed.
2. **Execution is gated.** `tools/call` requires
   `Authorization: Bearer <your CanaryUsers token>` **and** the account's MCP
   delivery toggle to be on (dashboard → CI & API). Disabled by default, so the
   endpoint is inert until you opt in.

> Quick sanity check: an anonymous `initialize` should return `200`; an anonymous
> `tools/call` should return `401`.

## Tools

| Tool | Input | Behavior |
|------|-------|----------|
| `canary_scan` | `url` (required), `depth`: `quick`\|`deep` | Runs a UX scan on the **deployed** URL. `quick` is a fast static check (free). `deep` renders the page, checks it visually on desktop + mobile, and clicks through key flows (~60–90s, uses a credit). Returns AI‑ready findings; a deep scan may return `running` with a `scanId` to poll. |
| `list_recent_scans` | `limit` (optional) | Lists the account's recent scans: id, URL, score, grade, status, date. |
| `get_report_markdown` | `scanId` (required) | Returns the full report for a scan id as Markdown (owner‑only). |

### Deep‑scan polling

A deep scan often takes longer than a single request. When it does, `canary_scan`
returns a `running` status with a `scanId`; the client should wait ~30s and call
`get_report_markdown(scanId)`, repeating until the report is ready.

## Discovery & verification

- **Official MCP Registry** listing: `ai.canaryusers/canaryusers`, verified via a DNS
  TXT record on `canaryusers.ai`.
- A discovery card is served at `/.well-known/mcp` and the domain‑verification key at
  `/.well-known/mcp-registry-auth` on the live site.

## Source of truth for metadata

`server.json` in this repo mirrors what the live server reports at `initialize`
(name + version) and what the registry lists. Pricing and plan numbers are **not**
duplicated here — see `https://www.canaryusers.ai/#pricing`.
