# Dubai Real Estate by DWI

Research Dubai property with registered figures instead of asking prices.
[Dubai Wealth Index](https://dubaiwealthindex.com) publishes median sale prices
from Dubai Land Department transactions, median rents from Ejari rental
contracts, and the gross rental yields between them, for apartment buildings,
areas and villa communities. Every figure carries its bedroom type, period, data
date and the number of transactions behind it.

This plugin connects Claude to that data over MCP and adds skills that tell
Claude how to quote it correctly.

## What you can ask

- "What does a 1-bedroom in Vera Tower, Business Bay rent for and yield?"
- "Which Dubai Marina buildings have the highest gross rental yields?"
- "Compare Vera Tower with 23 Marina for a 2-bedroom."
- "An agent says this building yields 9%. What do registered records support?"
- "Show my saved buildings and what changed since the last data edition."

## What is in here

| Path | |
| --- | --- |
| `.claude-plugin/plugin.json` | Claude plugin manifest |
| `.claude-plugin/marketplace.json` | Marketplace entry, so this repo can be added as a Claude Code marketplace |
| `.mcp.json` | The remote MCP server, `https://dubaiwealthindex.com/api/mcp` |
| `skills/` | Six skills: get started, cite a figure, building report, shortlist and compare, check a claim, track saved buildings |
| `plugin.json`, `mcp.json` | The same plugin in the [Agent Plugins](https://agent-plugins.org) format, for ChatGPT and other clients |
| `assets/` | Icon |

The skills are plain Markdown instructions. The plugin ships no scripts, hooks
or executables and installs no packages.

## Install

**Claude Code**

```bash
claude plugin marketplace add dubaiwealthindex/dubaiwealthindex-plugin
claude plugin install dubai-wealth-index@dubai-wealth-index
```

**Any MCP client, without the skills**

```bash
claude mcp add --transport http dubai-wealth-index https://dubaiwealthindex.com/api/mcp
```

On claude.ai, add `https://dubaiwealthindex.com/api/mcp` as a custom connector.

## Sign-in and the data it sends

Connecting opens a browser sign-in to your Dubai Wealth Index account (OAuth
2.1). There is no API key to copy. The plugin requests read access to property
data and, if you allow it, read access to the buildings you saved on the site.
Every tool is read-only: it cannot change your account or saved buildings.

The plugin talks to one server, `dubaiwealthindex.com`. When Claude calls a tool,
the tool's arguments, such as a building name, an area or a bedroom type, are
sent to that server. Dubai Wealth Index records each tool call in its product
analytics (PostHog): the tool name, its arguments, timing, errors, the AI
client's name and your account identifier. Nothing else from your conversation
or your machine is sent. See the [privacy policy](https://dubaiwealthindex.com/privacy).

## Scope

Dubai only. Figures describe registered transactions, not live listings or
individual-unit valuations. Villas are published per community. Short-term
rental figures use Airbnb asking rates and stated model assumptions. The plugin
cannot book viewings, contact agents, or give investment, mortgage, tax or legal
advice.

## Support

[dubaiwealthindex.com/contact](https://dubaiwealthindex.com/contact) or
hello@dubaiwealthindex.com. Report security issues as described in
[SECURITY.md](./SECURITY.md).
