---
name: feedback-default-scope-live-items-only
description: "For Laser Auto Parts catalog audits/tasks, default to counting and working with live/published items only, not hidden/unpublished ones, unless the user explicitly asks to include hidden items"
metadata: 
  node_type: memory
  pinned: false
  originSessionId: 1e18ce60-99d7-450c-97fc-e11ee1221e2c
  modified: 2026-09-15T19:37:13.932Z
---

When running any catalog audit, count, or fix pass on the Laser Auto Parts (Zid) store — SKU cleanup, year-range gaps, compatibility tables, model completeness, etc. — default the scope to **live/published products only**, not the full catalog including hidden/unpublished items. The owner corrected this directly ("u are counting hidden items only live items") after a stat was quoted from a full-catalog export (live + hidden) without filtering to published status first.

This mirrors a pattern already established project-specific compatibility-table work: the owner had explicitly scoped that work to "live products only" as the first/cheapest win, since hidden/unpublished items are a much larger and lower-priority set (the catalog has roughly 2,300+ total products but only ~800-900 are actually live/published at any given time). Apply this as the default assumption for any new counting or fix task on this catalog going forward — state the count as live-only, and call out explicitly if hidden items are also being included for some reason, rather than mixing the two silently.
