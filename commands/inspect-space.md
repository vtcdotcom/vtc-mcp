---
description: Inspect the current VTC Space's data via MCP
---

Inspect the signed-in VTC Space:

1. Call `explain_space_context` (Connect / sign in first if needed).
2. Call `space_db_info` and `list_collections` (with counts if useful).
3. For collections relevant to the question, `sample_document`, then `find_documents` with a narrow filter.
4. Summarize findings; never paste credentials or dump whole collections.
