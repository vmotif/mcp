# Vmotif MCP

Connect your agent to [Vmotif](https://vmotif.com), the node-based design canvas. Your agent can read and edit canvases, generate images, video, code, SVG, shaders and HTML screens, and run saved workflows. Every result lands on a canvas next to the prompt and references that made it.

This repository packages the hosted server for plugin marketplaces. The server itself runs at:

```
https://vmotif.com/api/mcp
```

Streamable HTTP, OAuth 2.1 sign-in on first use. Full docs and the tool reference: **https://vmotif.com/docs/mcp**

## Install

**Cursor** — [Add to Cursor](cursor://anysphere.cursor-deeplink/mcp/install?name=vmotif&config=eyJ1cmwiOiJodHRwczovL3Ztb3RpZi5jb20vYXBpL21jcCJ9), or add to `~/.cursor/mcp.json`:

```json
{ "mcpServers": { "vmotif": { "url": "https://vmotif.com/api/mcp" } } }
```

**Claude.ai / Claude Desktop** — Customize → Connectors → Add custom connector, paste the URL.

**Claude Code**

```sh
claude mcp add --transport http vmotif https://vmotif.com/api/mcp
```

**ChatGPT** — Plugins → Add → Add custom MCP server, paste the URL, choose OAuth.

**Codex**

```sh
codex mcp add --url https://vmotif.com/api/mcp --oauth-resource https://vmotif.com/api/mcp vmotif
codex mcp login vmotif
```

**Grok** — grok.com/connectors → New Connector → Custom, paste the URL.

## Plans and cost

Requires a Vmotif Pro, Team or Enterprise plan. Generations, captures, site scrapes and workflow runs spend your plan's GPU wallet; reading canvases, models and usage is free. If you belong to more than one workspace, the agent asks which one to bill.

## What's here

| File | For |
|---|---|
| `plugin.json`, `mcp.json` | Agent Plugins manifest (ChatGPT, Codex) |
| `.cursor-plugin/plugin.json` | Cursor Marketplace |
| `server.json` | Official MCP Registry (`com.vmotif/vmotif`) |
| `assets/` | Logos and icons |

## Support

https://vmotif.com/docs/support · Privacy: https://vmotif.com/privacy · Terms: https://vmotif.com/terms
