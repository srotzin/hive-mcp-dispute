# hive-mcp-dispute

[![srotzin/hive-mcp-dispute MCP server](https://glama.ai/mcp/servers/srotzin/hive-mcp-dispute/badges/score.svg)](https://glama.ai/mcp/servers/srotzin/hive-mcp-dispute)

[![Brand: Hive Civilization Gold](https://img.shields.io/badge/brand-%23C08D23-C08D23)](https://www.thehiveryiq.com)
[![License: MIT](https://img.shields.io/badge/license-MIT-blue)](LICENSE)
[![MCP: 2024-11-05](https://img.shields.io/badge/mcp-2024--11--05-blue)](https://modelcontextprotocol.io)

**Observational dispute/chargeback signal surface for agent-to-agent commerce.**

Hive does NOT arbitrate, judge, freeze funds, or enforce dispute outcomes. This is observational signal data only. Disputes are resolved by third-party protocols (Kleros, UMA, Reality.eth) or off-chain legal process. **Hive is not a court.**

## What this is

An MCP server that surfaces arbitration and reversal signals an agent can read **before** it agrees to a counterparty payment. It pulls from real public sources:

| Source | Surface | Method |
| --- | --- | --- |
| Kleros court-v2 subgraph | Active disputes, recent rulings | GraphQL (with on-chain `eth_getLogs` fallback against KlerosLiquid `0x988b…28069`) |
| UMA Optimistic Oracle v3 subgraph | Open assertions, disputed assertions | GraphQL (with on-chain fallback against `0xfb55…C0dE`) |
| Reality.eth subgraph | Open questions, pending arbitration | GraphQL (with on-chain fallback against `0xAf33…49CA`) |
| Etherscan / Basescan / Arbiscan | On-chain reversal-pattern detection (paired out-then-in within 24h, value within 5%, same peer) | REST `account/txlist` |

## What this is NOT

- ❌ A court
- ❌ An arbitrator
- ❌ A custodian of disputed funds
- ❌ A filing agent (no auto-filing with Kleros / UMA / Reality.eth)
- ❌ An enforcement layer

## Tools

| Tool | Description |
| --- | --- |
| `dispute_check` | Given counterparty address + chain → dispute history + active arbitration cases + on-chain reversal flags |
| `dispute_providers` | List Kleros / UMA / Reality.eth with current case load + intake URLs |
| `dispute_route` | Given case description + amount + jurisdiction → ranked provider options (no auto-filing) |
| `dispute_today` | 24h flagged-counterparty count + top providers by active case load |

## Endpoints (backend)

| Method | Path | Tool |
| --- | --- | --- |
| POST | `/v1/dispute/check`     | `dispute_check` |
| GET  | `/v1/dispute/providers` | `dispute_providers` |
| POST | `/v1/dispute/route`     | `dispute_route` |
| GET  | `/v1/dispute/today`     | `dispute_today` |

Every response carries the observational disclaimer and `brand_color: "#C08D23"`.

## Install

### Smithery (recommended for MCP clients)

```bash
npx -y @smithery/cli install hive-mcp-dispute --client claude
```

### Manual (local dev)

```bash
git clone https://github.com/srotzin/hive-mcp-dispute
cd hive-mcp-dispute
npm install
node server.js
```

The server listens on `:3000` by default and exposes:
- `POST /mcp` — JSON-RPC 2.0 / Streamable-HTTP (MCP 2024-11-05)
- `GET  /health` — liveness with disclaimer + brand color
- `GET  /.well-known/mcp.json` — discovery manifest

### Configure

| Env var | Default | Purpose |
| --- | --- | --- |
| `PORT` | `3000` | Listen port |
| `HIVE_BASE` | `https://hivemorph.onrender.com` | Backend root |

## Brand

Hive Civilization gold — Pantone 1245 C / `#C08D23`. Baked into every response.

## Disclaimer (canonical, baked into every response)

> Hive does not arbitrate, judge, freeze funds, or enforce dispute outcomes. This is observational signal data only. Disputes are resolved by third-party protocols (Kleros, UMA, Reality.eth) or off-chain legal process. Hive is not a court.

## License

MIT — see [LICENSE](LICENSE).

## Hive Civilization Directory

Part of the Hive Civilization — agent-native financial infrastructure.

- Endpoint Directory: https://thehiveryiq.com
- Live Leaderboard: https://hive-a2amev.onrender.com/leaderboard
- Revenue Dashboard: https://hivemine-dashboard.onrender.com
- Other MCP Servers: https://github.com/srotzin?tab=repositories&q=hive-mcp

Brand: #C08D23
<!-- /hive-footer -->
