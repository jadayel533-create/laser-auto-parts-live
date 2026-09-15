---
name: project-laser-auto-parts-zid-connector-outage
description: "Zid MCP connector OAuth outage 2026-09-12, RESOLVED 2026-09-14 — connector renamed to claude_ai_zidlaser and working normally"
metadata: 
  node_type: memory
  type: project
  modified: 2026-09-14T02:27:01.829Z
  originSessionId: 82f625ed-2ac9-4f60-9c2e-82dbb0421378
---

**RESOLVED 2026-09-14:** the connector is back and working. Note it now appears under a new tool namespace, `mcp__claude_ai_zidlaser__*` (was `mcp__claude_ai_Zid__*`), with per-resource tools (Products, ProductCategories, StoreLocations, Orders, etc.) instead of one generic Zid tool. Verified live via `StoreLocations` list call — returned the Jeddah AZYAR BRANCH location normally, no auth errors. No action needed going forward; just use the new tool names.

**2026-09-12: the "claude.ai Zid" MCP connector's OAuth sign-in is broken server-side, confirmed across two separate Claude accounts.** User reported a second Claude account couldn't sign in to the Zid connector, seeing "Couldn't register with zid's sign-in service. You can try again, or add an OAuth Client ID in the connector settings" with reference `ofid_ed2a505ba196c93d`. Tested directly in THIS session/account (the one all the [[project_laser_auto_parts]] work has been done through) via `mcp__claude_ai_Zid__authenticate` → tool reported connector not authenticated → ran `/mcp reconnect` → got the **identical error class**, different reference `ofid_7a8a8c040ca46369`.

**Conclusion: this is not an account-specific misconfiguration — it's a Dynamic Client Registration (DCR) failure on Zid's OAuth endpoint (`zam-mcp-server.zid.sa`) affecting all accounts.** The connector worked in every prior session for this project (Products/ProductCategories/ProductImages calls all succeeded historically), so this is a regression/outage, not a fundamental setup problem.

**How to apply:** if a future session finds the Zid connector unauthenticated/`needs_reconnect` and reconnect attempts fail with "Couldn't register with [sign-in service]... add an OAuth Client ID" — don't re-diagnose from scratch or assume it's account-specific. Try `/mcp reconnect` once or twice (transient outages do clear). If it still fails, the only real fixes are: (1) wait for Zid to fix their DCR endpoint, or (2) manually obtain an OAuth Client ID from wherever the Zid app/integration was registered (Zid Partner/Developer portal) and paste it into the connector settings in claude.ai's web UI — this cannot be done from the Claude Code CLI session itself, only in the web Settings → Connectors UI. Filed as product feedback to Anthropic this session (reference IDs above) since it's a clean two-account repro.
