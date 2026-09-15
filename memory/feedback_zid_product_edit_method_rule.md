---
name: feedback-zid-product-edit-method-rule
description: "Standing rule for Laser Auto Parts (Zid) — never create/edit products via Chrome click automation; use Excel bulk import instead, and use JS (not clicks) for any Chrome-based testing/verification"
metadata: 
  node_type: memory
  pinned: false
  originSessionId: 3cec27c5-4682-4a99-bb70-eab9668c34d7
  modified: 2026-09-15T03:47:35.785Z
---

The owner set this rule explicitly after watching a single product edit (adding a bilingual compatibility table to a product description, plus name/category changes) take about 40 minutes via Chrome click-based browser automation, with repeated undo/redo cycles caused by a floating hover toolbar in Zid's rich-text editor intercepting clicks meant for table cells. The owner's reaction ("omg stop", "so 1 edit 40 mins ok noted") led directly to this standing instruction — treat it as a hard rule, not just a one-off preference:

1. **Never create or edit Zid products by clicking through the dashboard UI in Chrome.** Always use Zid's native Excel Import/Export feature (Products page overflow menu) for actual product creates/edits, per the method already established in [[project_laser_auto_parts_bulk_import_export]]. This applies even to single-product edits, not just bulk changes — the click-based approach is banned regardless of how small the edit looks, because rich-text/table fields in the Zid dashboard are unreliable to drive via coordinate clicks (hover toolbars overlap cell boundaries, characters can drop during `type` actions, and Ctrl+Z/undo can cascade further than intended).
2. **Chrome browser automation is allowed only for testing/verification** — e.g., confirming how a saved product actually renders on the storefront, or checking dashboard state — never as the mechanism for making the actual data change.
3. **Whenever Chrome automation is used at all (including for testing), always use JS (`javascript_tool`) rather than simulated clicks/typing.** JS-based reads (e.g., querying `innerText` of a contenteditable field, or input `.value`) are far more reliable than screenshots for confirming exact state. Caveat learned the same session: JS can reliably *read* page state, but directly mutating a contenteditable rich-text editor's DOM via JS (e.g., setting `innerText` on a table cell) does NOT reliably persist — such editors often keep their own internal state model separate from the raw DOM, so a framework re-render (e.g., triggered by clicking Save) can silently revert the JS-injected change. JS is safe and preferred for verification; for any actual write inside such an editor, it must go through real UI interaction (or, per rule 1, not through Chrome at all — use the Excel import instead).
