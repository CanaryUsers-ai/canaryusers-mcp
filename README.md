# CanaryUsers — MCP Server

[![Official MCP Registry](https://img.shields.io/badge/MCP_Registry-ai.canaryusers%2Fcanaryusers-blue)](https://registry.modelcontextprotocol.io/v0/servers?search=canaryusers)
[![Smithery](https://img.shields.io/badge/Smithery-brettonb%2Fcanaryusers-7c3aed)](https://smithery.ai/servers/brettonb/canaryusers)
[![Website](https://img.shields.io/badge/setup-canaryusers.ai%2Fmcp-0aa)](https://www.canaryusers.ai/mcp)

> Give your AI coding assistant eyes for UX: a flock of AI users tests your deployed app and reports where real people would bail — each with a concrete fix.

**CanaryUsers** sends a flock of behaviorally‑diverse AI users through your **deployed** web app (a live or preview URL) and reports exactly where real people get stuck — confusing UX, broken mobile layouts, dead‑end conversion paths, and visual glitches — each with a concrete, AI‑ready fix you can apply in the same chat.

This is the public home for the **CanaryUsers remote MCP server**: manifest, install instructions, and docs. The product itself is hosted (closed source) — there's nothing to clone or run; you connect to the live endpoint. See [docs/WHY.md](docs/WHY.md) for why this repo exists and [docs/HOW-IT-WORKS.md](docs/HOW-IT-WORKS.md) for how the server works.

- **Registry name:** `ai.canaryusers/canaryusers`
- **Endpoint (remote, Streamable HTTP):** `https://www.canaryusers.ai/api/mcp`
- **Auth:** `Authorization: Bearer <your CanaryUsers token>`
- **Setup / get a token:** https://www.canaryusers.ai/mcp

## What it does

You point it at a URL; a flock of AI personas actually renders and explores the page like real users, then hands back prioritized, fix‑ready findings — not a vague score. Two modes:

- **Quick** (default) — a fast static check. Free.
- **Deep** — renders the page, inspects it **visually on desktop + mobile** (catches mobile breakage and layout issues), and **clicks through key flows** like signup / checkout / onboarding (~60–90s, uses credits).

## When to use it

- Right **after you ship or deploy a UI change** (it scans the deployed result, not your source).
- **Before a launch**, to pressure‑test the first‑run experience.
- To check a specific high‑stakes **flow**: signup, checkout, onboarding, pricing, or a landing page.
- When you suspect **mobile** breakage or a **conversion** drop‑off you can't see in analytics.

## Why not Lighthouse or analytics?

Lighthouse grades your HTML; analytics charts what already happened. CanaryUsers **sends users** and tells you, in plain language, *where they'd give up and why* — before real ones do.

## Tools

| Tool | What it does |
|------|--------------|
| `canary_scan` | Run a UX scan on a deployed URL. `depth`: `quick` (free, default) or `deep` (visual + flow, uses a credit). Returns AI‑ready findings. |
| `list_recent_scans` | List your recent scans (id, URL, score, grade, status, date). |
| `get_report_markdown` | Fetch the full report for a scan id as Markdown. |

## Install

You need a CanaryUsers token: **[canaryusers.ai/dashboard](https://www.canaryusers.ai/dashboard) → CI & API → toggle MCP on → copy your token.**

**Generic (Cursor `~/.cursor/mcp.json`, Windsurf, VS Code, Claude Desktop):**

```json
{
  "mcpServers": {
    "canaryusers": {
      "url": "https://www.canaryusers.ai/api/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_CANARYUSERS_TOKEN"
      }
    }
  }
}
```

**Claude Code (CLI):**

```bash
claude mcp add --transport http canaryusers https://www.canaryusers.ai/api/mcp \
  --header "Authorization: Bearer YOUR_CANARYUSERS_TOKEN"
```

Then just ask your assistant: *"Scan my app at https://… with CanaryUsers."*

## Pricing

**Quick scans are free. Deep scans use credits** (the credit model is roughly *1 credit = 1 scanned page*). Plans and monthly allotments change over time, so this repo intentionally does **not** hardcode the numbers — see the live pricing for current plans and limits:

➡️ **https://www.canaryusers.ai/#pricing** (single source of truth)

## Links

- **Setup & token:** https://www.canaryusers.ai/mcp
- **Pricing (live):** https://www.canaryusers.ai/#pricing
- **Official MCP Registry:** https://registry.modelcontextprotocol.io/v0/servers?search=canaryusers
- **Smithery:** https://smithery.ai/servers/brettonb/canaryusers

## License

[MIT](LICENSE) — applies to the contents of this metadata/docs repo. The CanaryUsers service itself is a separate hosted product.
