# Ferris EMU

Search software platform documentation powered by [Ferris AI](https://tryferris.app). This plugin provides semantic search across indexed platform docs directly from your IDE agent.

## What It Does

Gives your AI agent access to `search_software_context` — a tool that searches curated software platform documentation using semantic memory (Hindsight). Unlike generic web search, results come from structured, pre-indexed documentation with multi-strategy retrieval (semantic, keyword, graph, temporal).

## Install

### Cursor

**One-click install:**

[Install Ferris EMU in Cursor](cursor://anysphere.cursor-deeplink/mcp/install?name=ferris-emu&config=eyJ1cmwiOiJodHRwczovL2FwaS50cnlmZXJyaXMuYXBwL2V4dGVybmFsLW1jcC8ifQ%3D%3D)

Or install from the Cursor Marketplace: search for "ferris-emu" in Customize.

### Claude Code

```bash
claude mcp add --transport http --scope user ferris-emu https://api.tryferris.app/external-mcp/
```

Then authenticate by running `/mcp` in a Claude Code session and completing the OAuth flow.

## Authentication

This MCP server uses OAuth 2.1. On first connection, your IDE will prompt you to authenticate with your Ferris account. The OAuth flow opens your browser, you log in (or confirm if already logged in), and the token is stored locally.

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
| `bank` | string | Yes | Which platform to search (e.g. `qualtrics`, `enginehire`) |
| `limit` | integer | No | Max results (default: 10) |

## Development

To point at the dev environment instead of production:

**Cursor** — edit `mcp.json`:
```json
{
  "mcpServers": {
    "ferris-emu": {
      "url": "https://api.tryferris.dev/external-mcp/"
    }
  }
}
```

**Claude Code:**
```bash
claude mcp add --transport http --scope user ferris-emu https://api.tryferris.dev/external-mcp/
```

## Plugin Structure

```
ferris-emu-plugins/
├── .cursor-plugin/plugin.json     # Cursor plugin manifest
├── .claude-plugin/plugin.json     # Claude Code plugin manifest
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
