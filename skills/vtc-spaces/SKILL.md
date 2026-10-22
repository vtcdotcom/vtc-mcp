---
name: vtc-spaces
description: >-
  Use the VTC Spaces MCP server to search public Spaces and people, or, after OAuth sign-in,
  inspect a Space's data, list its apps, run confirmed AI Guide actions and draft workflows.
---

# VTC Spaces MCP

Each session is scoped to one VTC Space you belong to (or, without sign-in, to public data only).

## Connect

- Add `https://mcp.vtc.com/mcp` as a remote (Streamable HTTP) MCP server and click **Connect**.
  The browser opens VTC sign-in (email code) and a Space picker; tokens refresh automatically.
- Without signing in, the public/guest tools still work.
- Headless/CI: use a Space token from the app's Developer Help as `Authorization: Bearer <token>`.

## After connecting

1. `explain_space_context` or `get_space_summary` — confirm Space, role and environment
2. Prefer read tools first (`list_collections`, `list_space_apps`)
3. Writes (`execute_action`, `save_workflow`, `run_workflow`) need a `confirmation_token` from a prior proposal — show it to the human first

## Example prompts

- "Find public Spaces about photography"
- "List the collections in my Space"
- "What apps are installed in this Space?"
- "Propose a workflow from this description, then dry-run it"

## Tool map

| Goal | Tools |
|------|-------|
| Public discovery | `search_public`, `set_guest_context`, `list_guest_public_apps` |
| Space data | `list_collections` → `sample_document` / `find_documents` |
| Apps / API shapes | `list_space_apps`, `get_ai_manifest`, `list_developer_catalog` |
| Guide action | `list_available_actions` → `execute_action` (confirmed) |
| Workflow | `propose_workflow` → `save_workflow` / `run_workflow` (`dry_run=true` first) |
