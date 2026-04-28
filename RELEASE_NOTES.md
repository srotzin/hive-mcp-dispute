# hive-mcp-dispute — Release Notes

## v1.0.0 — 2026-04-28

Initial release. **Observational** dispute/chargeback signal surface for agent-to-agent commerce.

### Tools

- `dispute_check` — counterparty dispute history + on-chain reversal flags
- `dispute_providers` — Kleros / UMA / Reality.eth case load + intake URLs
- `dispute_route` — ranked arbitration provider options (NO auto-filing)
- `dispute_today` — 24h flagged-counterparty count + top providers by active load

### Real-rails data sources

- **Kleros** — court-v2 subgraph; on-chain `eth_getLogs` fallback against `KlerosLiquid` (`0x988b3A538b618C7A603e1c11Ab82Cd16dbE28069`) filtering on `DisputeCreation` topic
- **UMA Optimistic Oracle v3** — subgraph; on-chain fallback against `0xfb55F43fB9F48F63f9269DB7Dde3BbBe1ebDC0dE`
- **Reality.eth** — subgraph; on-chain fallback against `0xAf33DcB6E8c5c4D9dDF579f53031b514d19449CA`
- **Etherscan / Basescan / Arbiscan** — `account/txlist` for paired out-then-in reversal detection (24h window, value within 5%, same peer)

### Brand

Pantone 1245 C / `#C08D23` — baked into every response (`brand_color` field).

### Disclaimer (baked into every response)

> Hive does not arbitrate, judge, freeze funds, or enforce dispute outcomes. This is observational signal data only. Disputes are resolved by third-party protocols (Kleros, UMA, Reality.eth) or off-chain legal process. Hive is not a court.

### What this release does NOT do

- ❌ No arbitration or judgment
- ❌ No fund freezing or custody
- ❌ No enforcement of outcomes
- ❌ No auto-filing with Kleros / UMA / Reality.eth
- ❌ No HiveLaw escrow (permanently rejected)
