# VTC Spaces MCP

Connect Cursor, Claude, ChatGPT, VS Code or any MCP client to [VTC](https://vtc.com) Spaces.

- **Without signing in:** search public Spaces, networks and people, browse a Space's public apps and public collections.
- **After OAuth sign-in:** work inside a Space you belong to: inspect its data (read-only), list installed apps and their AI manifests, run AI Guide actions after an explicit confirmation step, and draft, save or dry-run Space workflows.

| | |
|---|---|
| Endpoint | `https://mcp.vtc.com/mcp` |
| Transport | Streamable HTTP |
| Auth | OAuth 2.1 (PKCE, dynamic client registration) via `https://api.vtc.com`; guest mode for public tools |
| Registry | [`com.vtc/mcp`](https://registry.modelcontextprotocol.io/v0.1/servers?search=com.vtc/mcp) in the official MCP Registry |
| Docs | https://vtc.com/developers |

This repository holds the client configuration, Cursor rules, skills and commands for the hosted server. It does not contain the server source.

## Install

### Cursor

[![Add VTC Spaces MCP to Cursor](https://cursor.com/deeplink/mcp-install-dark.svg)](https://cursor.com/en-US/install-mcp?name=vtc-spaces&config=eyJ1cmwiOiJodHRwczovL21jcC52dGMuY29tL21jcCJ9)

Or add it to `~/.cursor/mcp.json` (or the project's `.cursor/mcp.json`):

```json
{
  "mcpServers": {
    "vtc-spaces": { "type": "http", "url": "https://mcp.vtc.com/mcp" }
  }
}
```

Then open **Settings → MCP**, and click **Connect** next to *vtc-spaces* to sign in.

### VS Code / GitHub Copilot

```json
{ "servers": { "vtc-spaces": { "type": "http", "url": "https://mcp.vtc.com/mcp" } } }
```

### Claude, ChatGPT and other clients

Add a custom connector with the URL `https://mcp.vtc.com/mcp`. Clients that support MCP OAuth open a VTC sign-in page.

## Sign-in

1. Your client opens VTC in the browser.
2. Enter your email and the one-time code VTC sends you.
3. Pick the Space this connection may use.
4. You return to your client. Tokens refresh automatically and can be revoked from VTC.

Each connection is scoped to **one Space**. Tools can't read other Spaces.

**Headless / CI:** generate a Space token in your app's *Developer Help* and send it as `Authorization: Bearer <token>`.

## Tools (29)

R = read-only · W = changes state · D = destructive/consequential; always requires a confirmation token first

### Public (no sign-in)

| Tool | Type | What it does |
|---|---|---|
| `search_public` | R | Search public people, Spaces and networks by name or username |
| `search_public_spaces` | R | Find public Spaces by name or slug |
| `search_public_people` | R | Find public people by username (email is not searchable) |
| `set_guest_context` | R | Select one public Space for this session |
| `list_guest_public_apps` | R | List a Space's apps that allow guest browsing |
| `describe_public_app_surface` | R | AI manifest and public read hints for one app |
| `list_public_collection_documents` | R | Read documents from collections a Space made public |
| `fetch_app_public_route` | R | GET an allow-listed public route of a Space app on `*.apps.vtc.com` |
| `list_developer_catalog` | R | Public catalog of VTC platform APIs for app developers |

### Sign-in and session

| Tool | Type | What it does |
|---|---|---|
| `login_with_otp` | W | Send a one-time sign-in code to your email (fallback when OAuth isn't available) |
| `verify_otp` | W | Complete code sign-in for a Space |
| `set_session_jwt` | W | Use a Space token for this session (kept in memory only) |
| `validate_jwt` | R | Show the current token's claims |
| `explain_space_context` | R | Summarize Space, user, role and environment; call this first |
| `get_space_summary` | R | Lightweight Space summary from the token |

### Space data (read-only)

| Tool | Type | What it does |
|---|---|---|
| `space_db_info` | R | Describe the Space's data store |
| `list_collections` | R | List collections |
| `count_documents` | R | Count documents matching a filter |
| `find_documents` | R | Find up to 50 documents with filter and projection |
| `sample_document` | R | One document for schema inference; sensitive fields redacted |

### Apps and actions

| Tool | Type | What it does |
|---|---|---|
| `list_space_apps` | R | Apps installed in the Space |
| `get_ai_manifest` | R | AI tool manifest for an installed app |
| `list_available_actions` | R | AI Guide actions your role may run |
| `execute_action` | D | Run an AI Guide action; needs a confirmation token |

### Workflows

| Tool | Type | What it does |
|---|---|---|
| `list_workflows` | R | List saved workflows |
| `get_workflow` | R | Get one workflow's steps and trigger |
| `propose_workflow` | R | Draft a workflow from natural language (not saved) |
| `save_workflow` | D | Save a workflow (Space admin); needs a confirmation token |
| `run_workflow` | D | Run a workflow; dry run by default, a real run needs a confirmation token |

## What's in this repo

| Path | Purpose |
|---|---|
| `mcp.json` | Remote MCP server config for Cursor plugin installs (`.mcp.json` is the same config for other clients) |
| `.cursor-plugin/plugin.json` | Plugin metadata |
| `rules/vtc-spaces.mdc` | Cursor rule: safe usage (read-only data, confirmations, one Space) |
| `skills/vtc-spaces/SKILL.md` | Agent skill: how to connect and use the tools |
| `commands/` | Slash-command prompts |

## Support

- Docs: https://vtc.com/developers
- Contact: https://vtc.com/contact
- Privacy: https://vtc.com/privacy · Terms: https://vtc.com/terms

## License

Licensed under the [Apache License 2.0](LICENSE).
