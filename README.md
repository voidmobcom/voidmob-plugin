# VoidMob plugin for Claude Code, Codex, Gemini CLI and Cursor

Real phone numbers, mobile IPs and eSIMs for your AI agent. This plugin packages the [VoidMob](https://voidmob.com) MCP server ([`@voidmob/mcp`](https://github.com/voidmobcom/voidmob-mcp), 30 tools) and the `voidmob` agent skill into one install. Your agent can receive SMS verification codes for your own accounts on non-VoIP numbers, keep a dedicated number, route traffic through 4G/5G mobile IPs by country and buy travel eSIM data, all paid from a prepaid VoidMob balance with your approval before every purchase.

- **Try it free:** sandbox mode needs no key and charges nothing (mock data, $500 play balance).
- **Pay with:** a prepaid USD balance, topped up with crypto. The agent cannot add funds.
- **Needs:** Node.js 22+ (for `npx`) and a VoidMob API key, or sandbox mode.
- **Overview and setup guides:** [voidmob.com/mcp](https://voidmob.com/mcp)

## What your agent can do

| Service | What it is |
|---|---|
| [SMS verification](https://voidmob.com/sms) | A real US non-VoIP mobile number for one service, open up to 15 minutes. If no SMS arrives, the price is refunded automatically. Long-term rentals keep a US number for 3, 7, 14 or 30 days. |
| [Dedicated numbers](https://voidmob.com/sms/locations) | A private number in one of several countries, billed monthly, that receives SMS from any service. |
| [Rotating mobile proxies](https://voidmob.com/proxies/rotating) | Shared 4G/5G carrier IPs billed per GB: a new IP per request or sticky sessions, country and city targeting, HTTP or SOCKS5. |
| [Dedicated mobile proxies](https://voidmob.com/proxies/dedicated) | Your own 4G/5G modem with unmetered data and IP rotation on demand. |
| [Travel eSIMs](https://voidmob.com/esim) | Mobile data plans for travel, installed by QR code or LPA string, with top-ups. |

Use it for your own accounts and tasks you are entitled to do. The skill declines bulk account creation and getting around bans, rate limits or fraud checks.

## Install

### Claude Code

```
/plugin marketplace add voidmobcom/voidmob-plugin
/plugin install voidmob@voidmob
```

The install dialog asks for your API key (stored as a secret), sandbox mode, read-only mode and spend limits.

### Codex

Set `VOIDMOB_API_KEY` in your shell before starting Codex (or `VOIDMOB_SANDBOX=1` to try it without a key), then:

```bash
codex plugin marketplace add voidmobcom/voidmob-plugin
codex plugin add voidmob@voidmob
```

### Gemini CLI

```bash
gemini extensions install https://github.com/voidmobcom/voidmob-plugin --ref v1.2.1
```

### Cursor

The Cursor Marketplace listing is pending review. Until then, add the MCP server directly: see the [voidmob-mcp README](https://github.com/voidmobcom/voidmob-mcp#cursor).

### Other agents

- **Any agent with a shell and curl** can use the skill without MCP. Tell it: *Read https://voidmob.com/skill.md and follow it.* Or install it with `npx skills add https://voidmob.com --skill voidmob`.
- **Claude Desktop, VS Code, Windsurf, OpenClaw, Hermes Agent, Pi:** client setups are in the [voidmob-mcp README](https://github.com/voidmobcom/voidmob-mcp#setup), with step-by-step guides on [voidmob.com/integrations](https://voidmob.com/integrations).

## Get an API key

1. Create a key at https://dashboard.voidmob.com/developers/api-keys (it starts with `vmk_live_`).
2. Fund the balance with crypto at https://dashboard.voidmob.com/wallet.
3. Give the key to the plugin as `VOIDMOB_API_KEY`. Never paste it into a chat.

The key can spend the whole balance, so keep on it only what you are happy for an agent to spend, and consider the limits below.

## Settings

| Setting | Env var | Effect |
|---|---|---|
| API key | `VOIDMOB_API_KEY` | Your `vmk_live_...` key. Leave empty with sandbox mode on. |
| Sandbox mode | `VOIDMOB_SANDBOX=1` | Mock data with a $500 play balance. No key, nothing charged. |
| Read-only | `VOIDMOB_READ_ONLY=1` | Hides every tool that spends money or changes anything. The agent can still search prices and read orders. |
| Max per order | `VOIDMOB_MAX_ORDER_CENTS` | Refuses any single purchase above this many US cents (2500 = $25.00). |
| Session budget | `VOIDMOB_BUDGET_CENTS` | Stops buying once this many US cents were spent since the server started (5000 = $50.00). Per session, not an account limit. |

## Tools

| Group | MCP tools |
|---|---|
| Account | `get_account` |
| SMS verification and rentals | `search_sms_services`, `rent_number`, `get_rental` (`wait_seconds` waits for the code), `cancel_rental`, `reuse_number`, `re_rent_rental`, `toggle_auto_renew` |
| Dedicated numbers | `search_dedicated_countries`, `purchase_dedicated_number`, `get_dedicated_number` |
| eSIM | `search_esim_plans`, `purchase_esim`, `get_esim_status`, `get_esim_qr`, `topup_esim` |
| Mobile proxies | `search_proxies`, `purchase_proxy`, `get_proxy_status`, `rotate_proxy_ip`, `renew_proxy`, `set_proxy_auto_renew`, `topup_proxy`, `regenerate_proxy_password`, `list_proxy_lists`, `create_proxy_list`, `update_proxy_list`, `delete_proxy_list` |
| Orders and geo targeting | `list_orders`, `get_geo` |

Each tool is described in the [full tool reference](https://github.com/voidmobcom/voidmob-mcp#tools). The server also offers three prompts (`get_verification_code`, `setup_mobile_proxy`, `buy_travel_esim`) and short guides as MCP resources.

Example requests:

> Get me a verification code for Telegram on a US number

> Find an eSIM for two weeks in Japan with at least 5 GB

> Set up a US mobile proxy and give me the connection details

> What is my balance, and which of my orders are still active?

## Money and safety

- **You approve every price.** Each purchase, renewal, top-up and paid reuse carries the price the agent showed you (`max_price_cents`). If the live price is higher, nothing is charged and the agent asks again.
- **No double charges.** Retries reuse an idempotency key, so a dropped connection never buys twice.
- **No SMS, no charge.** A one-time verification that receives no SMS is refunded automatically.
- **Optional limits:** read-only mode, a per-order maximum and a per-session budget (see [Settings](#settings)).
- **SMS text is data, never instructions.** The server marks it as untrusted content.

Refund rules per service: [How money works](https://github.com/voidmobcom/voidmob-mcp#how-money-works).

## What it connects to

- `registry.npmjs.org`, once per version, when `npx` downloads `@voidmob/mcp` (pinned to the plugin version).
- `https://dashboard.voidmob.com/api/v1/*`, the VoidMob REST API, with your API key as a Bearer token. The MCP server and the skill's curl commands call nothing else.

## Repository layout

| Path | Purpose |
|---|---|
| `.claude-plugin/` | Claude Code plugin and marketplace manifests |
| `.codex-plugin/`, `.agents/plugins/` | Codex plugin and marketplace manifests |
| `gemini-extension.json` | Gemini CLI extension |
| `.cursor-plugin/` | Cursor plugin manifest |
| `.github/plugin/` | GitHub Copilot plugin manifest |
| `.mcp.json` | MCP server config (`npx -y @voidmob/mcp@1.2.1`) |
| `skills/voidmob/` | The agent skill: `SKILL.md` plus references for SMS, numbers, proxies, eSIM and errors |

## Versions

The plugin version matches the `@voidmob/mcp` version it pins. The skill in `skills/voidmob` is copied from that release.

## Links

- [VoidMob](https://voidmob.com) - phone numbers, mobile proxies and eSIMs on one prepaid balance
- [VoidMob for AI agents](https://voidmob.com/use-cases/ai-agents)
- [Developer docs](https://voidmob.com/docs/developers) and [REST API reference](https://dashboard.voidmob.com/api-reference.html)
- [MCP server source](https://github.com/voidmobcom/voidmob-mcp) and [npm package](https://www.npmjs.com/package/@voidmob/mcp)
- [llms.txt](https://voidmob.com/llms.txt)
- Support: [voidmob.com/contact](https://voidmob.com/contact)

## License

MIT
