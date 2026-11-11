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

**Skill** — once connected, teach the agent the working loop with the [Vmotif skill](https://github.com/vmotif/skills): `npx skills add vmotif/skills`.

## Plans and cost

Requires a Vmotif Pro, Team or Enterprise plan. Generations, captures, site scrapes and workflow runs spend your plan's GPU wallet; reading canvases, models and usage is free. If you belong to more than one workspace, the agent asks which one to bill.

## Tools

The server exposes 26 tools. **Read-only** tools never change anything; **Writes** tools create or edit canvas content; **Destructive** tools can replace or remove existing content. Tools marked *Spends wallet* draw on your plan's GPU wallet. Parameters and return shapes: https://vmotif.com/docs/mcp

<!-- tools:start -->
| Tool | Title | Access | Spends wallet |
|---|---|---|---|
| `add_canvas_nodes` | Add canvas nodes | Writes |  |
| `add_generation_to_project` | Add generation to project | Writes |  |
| `add_premade_workflow` | Add premade workflow | Writes |  |
| `create_canvas` | Create canvas | Writes |  |
| `create_image_upload` | Create image upload | Writes |  |
| `generate_code` | Generate code | Writes | Yes |
| `generate_image` | Generate image | Writes | Yes |
| `generate_video` | Generate video | Writes | Yes |
| `get_favorite_projects` | Get favorite projects | Read-only |  |
| `get_generation` | Get generation | Read-only |  |
| `get_node_status` | Get node status | Read-only |  |
| `get_usage` | Get usage | Read-only |  |
| `get_workflow` | Get workflow | Read-only |  |
| `get_workflow_run` | Get workflow run | Read-only |  |
| `list_client_brands` | List brands | Read-only |  |
| `list_models` | List models | Read-only |  |
| `list_premade_workflows` | List premade workflows | Read-only |  |
| `list_projects` | List projects | Read-only |  |
| `list_workflows` | List workflows | Read-only |  |
| `list_workspaces` | List workspaces | Read-only |  |
| `read_canvas` | Read canvas | Read-only |  |
| `run_capture` | Run capture | Writes | Yes |
| `run_site_scrape` | Run site scrape | Destructive | Yes |
| `run_workflow` | Run workflow | Writes | Yes |
| `transform_image` | Transform image | Writes |  |
| `update_canvas` | Update canvas | Destructive |  |
<!-- tools:end -->

## Events

Instead of polling `get_generation` or `get_workflow_run`, an agent can subscribe to be told when work settles. The server implements the [MCP Events draft](https://github.com/modelcontextprotocol/experimental-ext-triggers-events) (`events/list`, `events/subscribe`, `events/unsubscribe`) with webhook delivery: each event is one signed POST to the URL you name, with [Standard Webhooks](https://www.standardwebhooks.com/) headers (`webhook-id`, `webhook-timestamp`, `webhook-signature`) and `X-MCP-Subscription-Id`. The URL is challenged once before a subscription is accepted, deliveries retry with backoff for a day, and a subscription expires unless it is refreshed (re-subscribe with the same name, arguments and URL). The [AI SDK](https://ai-sdk.dev/docs/ai-sdk-core/mcp-events) receives them with `experimental_createMCPEventWebhook`. Details, filters and payload fields: https://vmotif.com/docs/mcp#events

<!-- events:start -->
| Event | When |
|---|---|
| `generation.completed` | A generation this account started finished and its result is stored. Read it with get_generation; the asset is also placed on its canvas. |
| `generation.failed` | A generation this account started failed, was abandoned by the queue, or was swept as stuck. A failed job is not charged unless the model had already produced output. |
| `workflow_run.completed` | A headless workflow run (run_workflow, or the public API) finished with every output settled. Read the results with get_workflow_run. |
| `workflow_run.failed` | A headless workflow run ended with a failure. Outputs that had completed are kept on the canvas. |
<!-- events:end -->

## Agent Plugins compatibility

`mcp.json` includes OpenAI's documented `com.openai` OAuth extension. The generic Agent Plugins 1.0.0 schema rejects that extension, so strict portable clients should use `.mcp.json` instead. It targets the same server and uses standard OAuth discovery. Cursor also references `.mcp.json`.

## What's here

| File | For |
|---|---|
| `plugin.json`, `mcp.json` | Agent Plugins manifest with OpenAI OAuth metadata (ChatGPT and portable Codex plugins) |
| `.mcp.json` | Schema-valid portable config for project-scoped and strict Agent Plugins clients (Claude Code, Codex CLI, Cursor) |
| `.cursor-plugin/plugin.json` | Cursor Marketplace |
| `server.json` | Official MCP Registry (`com.vmotif/vmotif`) |
| `assets/` | Logos and icons |

## Support

https://vmotif.com/docs/support · Privacy: https://vmotif.com/privacy · Terms: https://vmotif.com/terms

## Contributing

This repository is published from Vmotif's private monorepo, so pull requests here can't be merged. To report a problem or suggest a change, open an issue or contact [support](https://vmotif.com/docs/support).
