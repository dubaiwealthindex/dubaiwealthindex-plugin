# Dubai Real Estate Data — Dubai Wealth Index plugin

Dubai real estate data for Claude, ChatGPT, Cursor and Grok: registered
property sale prices, rents and rental yields for every published Dubai
apartment building, area and villa community.

[Dubai Wealth Index](https://dubaiwealthindex.com) publishes median sale prices
from Dubai Land Department (DLD) transactions, median rents from Ejari rental
contracts, and the gross rental yields between them. These are recorded
prices, not asking prices or listings. Every figure carries its bedroom type,
period, data date and the number of transactions behind it.

This plugin connects your AI assistant to that data over MCP and adds skills
that tell it how to quote the figures correctly.

## What you can ask

- "What does a 1-bedroom in Vera Tower, Business Bay rent for and yield?"
- "Which Dubai Marina buildings have the highest gross rental yields?"
- "Compare Vera Tower with 23 Marina for a 2-bedroom."
- "An agent says this building yields 9%. What do registered records support?"
- "Is AED 2.35M a fair price for a 2-bedroom in Executive Tower G? What should I offer?"
- "Is AED 5.6M fair for a 3-bed Palmera villa in Arabian Ranches?"
- "Show my saved buildings and what changed since the last data edition."

## Tools and prompts

The server at `https://dubaiwealthindex.com/api/mcp` serves 26 tools and 5
prompts. Every tool reads, except `request_info`.

| | Tools |
| --- | --- |
| Find and read records | `search`, `fetch`, `get_building`, `get_building_transactions`, `get_area`, `get_villa_community`, `get_city_overview` |
| Prices, screens and rankings | `check_property`, `compare_buildings`, `screen_buildings`, `get_ranking`, `list_rankings` |
| Off-plan and developers | `get_project`, `get_offplan`, `get_developer`, `get_price_index`, `get_supply_pipeline` |
| Market and costs | `get_market_activity`, `get_market_signals`, `get_commercial`, `get_service_charge_budgets` |
| Definitions and editions | `get_methodology`, `whats_new` |
| Your account (read) | `whoami`, `list_favorites` |
| Enquiry (writes, after you confirm) | `request_info` |

Prompts: `evaluate_building`, `compare_areas`, `find_high_yield`,
`short_let_vs_lease`, `cite_figure`.

## What is in here

| Path | |
| --- | --- |
| `.claude-plugin/plugin.json` | Claude plugin manifest |
| `.claude-plugin/marketplace.json` | Marketplace entry, so this repo can be added as a Claude Code marketplace |
| `.mcp.json` | The remote MCP server, `https://dubaiwealthindex.com/api/mcp` |
| `skills/` | Nine skills: get started, cite a figure, building report, shortlist and compare, check a claim, check an asking price, track saved buildings, short-term rental, request info |
| `plugin.json`, `mcp.json` | The same plugin in the [Agent Plugins](https://agent-plugins.org) format, for ChatGPT, Codex and other clients |
| `.codex-plugin/plugin.json` | Codex fallback manifest; the OpenAI settings in `plugin.json` take precedence |
| `.cursor-plugin/` | Cursor manifest and marketplace entry |
| `.grok-plugin/` | Grok manifest |
| assets/ | Icon and logo |

The skills are plain Markdown instructions. The plugin ships no scripts, hooks
or executables and installs no packages.

## Install

**Claude Code**

```bash
claude plugin marketplace add dubaiwealthindex/dubaiwealthindex-plugin
claude plugin install dubai-wealth-index@dubai-wealth-index
```

**Cursor** — add this repository from the Cursor plugin marketplace, or point
Cursor at it as a plugin repository.

**Grok** — `grok` loads it from `~/.grok/plugins/`, or pass the cloned
repository with `--plugin-dir`.

**ChatGPT and Codex** — use the root `plugin.json` and `mcp.json`
(`.codex-plugin/plugin.json` is the Codex fallback).

**Any MCP client, without the skills**

```bash
claude mcp add --transport http dubai-wealth-index https://dubaiwealthindex.com/api/mcp
```

On claude.ai, add `https://dubaiwealthindex.com/api/mcp` as a custom connector.

## Sign-in and the data it sends

Connecting opens a browser sign-in to your Dubai Wealth Index account (OAuth
2.1). There is no API key to copy. The plugin requests read access to property
data and, if you allow it, read access to the buildings you saved on the site.

25 of the 26 tools only read. `request_info` is the one that writes, and only
after you confirm: it stores an enquiry with Dubai Wealth Index holding your
account's name and email, the property, any bedroom type, message and phone
number you give, and the name of the AI app that sent it. Dubai Wealth Index
replies by phone, WhatsApp or email and puts you in touch with a licensed agent,
so the enquiry may be passed to the licensed brokerage it works with. It cannot
change your account or saved buildings.

The plugin talks to one server, `dubaiwealthindex.com`. When the assistant calls
a tool, the tool's arguments, such as a building name, an area or a bedroom
type, are sent to that server. Dubai Wealth Index records each tool call in its
product analytics (PostHog): the tool name, its arguments, timing, errors, the
size of the result, the AI app's name, the connection's client ID and your
account identifier. The name, phone number and message given to `request_info`
are left out of analytics, and tool results are not recorded. Nothing else from
your conversation or your machine is sent. See the
[privacy policy](https://dubaiwealthindex.com/privacy).

## Scope

Dubai only. Figures describe registered transactions, not live listings. An
asking price is checked against registered sales of similar units: in the same
building for an apartment, in the same community (and phase) for a villa.
Villas are published per community. Short-term
rental figures use Airbnb asking rates and stated model assumptions. To buy,
rent or view, the plugin sends your enquiry to Dubai Wealth Index, which replies
and introduces a licensed agent. It gives no investment, mortgage, tax or legal advice.

## Support

[dubaiwealthindex.com/contact](https://dubaiwealthindex.com/contact) or
hello@dubaiwealthindex.com. Report security issues as described in
[SECURITY.md](./SECURITY.md).
