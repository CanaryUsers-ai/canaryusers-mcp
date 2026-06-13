# Why this repo exists

_Last updated: 2026-06-09._

CanaryUsers is a **hosted, closed‑source product**. The application code lives in a
separate **private** repository. That created friction for MCP distribution,
because a lot of the discovery ecosystem assumes a **public GitHub repo**:

- **awesome‑mcp‑servers** only accepts servers that link to a `https://github.com/…`
  repository (it auto‑rejects non‑GitHub links).
- **Glama** and several indexers auto‑pull metadata / run checks from a public repo
  (some even want a Dockerfile to boot the server).
- Humans and AIs browsing for a server expect a README to read.

We don't want to open‑source the whole app just to get listed. So this repo,
**`canaryusers-mcp`**, is the public‑facing surface for the MCP server only:

- `server.json` — the canonical remote‑server manifest.
- `README.md` — the human/AI‑readable description, install steps, and links.
- `docs/` — how it works and why these decisions were made.

There is **no app code here** — the server is the live hosted endpoint at
`https://www.canaryusers.ai/api/mcp`.

## How the server is published (the canonical listing)

The authoritative listing is the **official MCP Registry**
(`registry.modelcontextprotocol.io`), under the custom‑domain namespace
**`ai.canaryusers/canaryusers`**. We use the domain namespace (not `io.github.*`)
precisely because the source repo is private — ownership is proven by verifying the
`canaryusers.ai` domain instead of a public GitHub repo.

Domain verification uses **DNS** (a TXT record on `canaryusers.ai`), not HTTP,
because the apex 308‑redirects to `www` and the registry won't follow the redirect.
Most other directories (PulseMCP, Glama, many MCP clients) **sync from the official
registry**, so getting that one right propagates downstream automatically.

## Keeping copy from going stale

Listings (registry, Smithery, this README) are **static snapshots** — they do not
auto‑update when the product changes. To avoid drift, especially on **pricing**:

- Describe the *model* ("quick scans free; deep scans use credits"), not the numbers.
- Link to the **live source of truth** — `https://www.canaryusers.ai/#pricing` — for
  current plans and allotments.

If the product's name, endpoint, or tools change, update `server.json` here and
re‑publish to the registry (and re‑run the Smithery publish); see the private repo's
`docs/mcp-registry-publish.md` for the exact commands.
