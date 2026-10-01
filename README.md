# VoidMob plugin

The [VoidMob MCP server](https://github.com/voidmobcom/voidmob-mcp) (`@voidmob/mcp`, 30 tools) and the `voidmob` agent skill, packaged as a plugin for Claude Code, Codex, Gemini CLI and Cursor. Your agent can receive SMS verification codes for your own accounts on US non-VoIP numbers (refunded automatically if no SMS arrives), keep a dedicated number, buy eSIM data plans for travel and use 4G/5G mobile IPs by country, paid from a prepaid VoidMob balance.

## Install

- **Claude Code:** `/plugin marketplace add voidmobcom/voidmob-plugin`, then `/plugin install voidmob@voidmob`. The dialog asks for your API key (stored as a secret), sandbox mode, read-only mode and spend limits.
- **Codex:** export `VOIDMOB_API_KEY` (or `VOIDMOB_SANDBOX=1`), then `codex plugin marketplace add voidmobcom/voidmob-plugin` and `codex plugin add voidmob@voidmob`.
- **Gemini CLI:** `gemini extensions install https://github.com/voidmobcom/voidmob-plugin --ref v1.2.1`
- **Cursor:** the Cursor Marketplace listing is pending review. Until then, add the MCP server directly: see the [voidmob-mcp README](https://github.com/voidmobcom/voidmob-mcp#cursor).

Create a key at https://dashboard.voidmob.com/developers/api-keys and fund the balance with crypto at https://dashboard.voidmob.com/wallet. Sandbox mode needs no key and charges nothing.

## Money and safety

Every purchase needs the price the agent showed you (`max_price_cents`); a higher live price charges nothing. Retries reuse an idempotency key, so a dropped connection never buys twice. Optional limits: read-only mode, a per-order maximum and a per-session budget. Details: [How money works](https://github.com/voidmobcom/voidmob-mcp#how-money-works).

## What it connects to

- `registry.npmjs.org`, once per version, when `npx` downloads `@voidmob/mcp` (pinned to the plugin version).
- `https://dashboard.voidmob.com/api/v1/*`, the VoidMob REST API, with your API key as a Bearer token. The MCP server and the skill's curl commands call nothing else.

## Versions

The plugin version matches the `@voidmob/mcp` version it pins. The skill in `skills/voidmob` is copied from that release.

## License

MIT
