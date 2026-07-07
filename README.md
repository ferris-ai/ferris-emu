# Ferris EMU

Search software platform documentation powered by [Ferris AI](https://tryferris.ai). This plugin provides semantic search across indexed platform docs directly from your AI assistant or coding agent.

## What It Does

Gives your AI agent access to `search_software_context` — a tool that searches curated software platform documentation using semantic memory (Hindsight). Unlike generic web search, results come from structured, pre-indexed documentation with multi-strategy retrieval (semantic, keyword, graph, temporal).

## Install

### Claude.ai

1. Go to **Settings > Connectors > Add custom connector**
2. Paste the server URL: `https://api.tryferris.app/emu/mcp`
3. Complete the OAuth flow to connect your Ferris account

Requires a Claude Pro, Max, Team, or Enterprise plan.

### ChatGPT

1. Go to **Settings > MCP** (or search for MCP servers)
2. Add a custom MCP server with the URL: `https://api.tryferris.app/emu/mcp`
3. Complete the OAuth flow to connect your Ferris account

### Cursor

**One-click install:**

[Install Ferris EMU in Cursor](cursor://anysphere.cursor-deeplink/mcp/install?name=ferris-emu&config=eyJ1cmwiOiJodHRwczovL2FwaS50cnlmZXJyaXMuYXBwL2VtdS9tY3AifQ%3D%3D)

Or install from the Cursor Marketplace: search for "ferris-emu" in Customize.

### Claude Code

**Via plugin marketplace:**

```
/plugin marketplace add ferris-ai/ferris-emu
/plugin install ferris-emu@ferris-emu
```

**Or add the MCP server directly:**

```bash
claude mcp add --transport http --scope user ferris-emu https://api.tryferris.app/emu/mcp
```

Then authenticate by running `/mcp` in a Claude Code session and completing the OAuth flow.

## Authentication

This MCP server uses OAuth 2.1. On first connection, your AI assistant will prompt you to authenticate with your Ferris account. The OAuth flow opens your browser, you log in (or confirm if already logged in), and the token is stored automatically.

## Usage

Once installed and authenticated, your agent can search platform documentation:

```
Search for how to configure Qualtrics survey distribution via API
```

The agent will automatically use the `search_software_context` tool. See `skills/search-software-context/SKILL.md` for detailed query optimization tips.

## Tool Reference

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `query` | string | Yes | Natural language search query |
| `bank` | enum | Yes | Which platform to search — dynamically populated from active banks on the server |
| `limit` | integer | No | Max results (default: 10) |

The `bank` parameter is a **dynamic enum** — the server populates valid values at runtime based on which software documentation banks are active for your project. Current options include `qualtrics`, `enginehire`, and `salesforce-developer`.

## Plugin Structure

```
ferris-emu-plugins/
├── .cursor-plugin/plugin.json     # Cursor plugin manifest
├── .claude-plugin/
│   ├── plugin.json                # Claude Code plugin manifest
│   └── marketplace.json           # Claude Code marketplace catalog
├── skills/
│   └── search-software-context/
│       └── SKILL.md               # Agent skill with search optimization guidance
├── mcp.json                       # Cursor MCP server config
├── .mcp.json                      # Claude Code MCP server config
├── assets/
│   └── logo.svg                   # Plugin logo
└── README.md
```

## License

MIT
