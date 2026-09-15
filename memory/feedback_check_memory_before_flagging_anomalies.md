---
name: feedback-check-memory-before-flagging-anomalies
description: "Cross-check own prior documented findings before declaring something an unexplained bug/anomaly — caught re-flagging the Laser Auto Parts category count inflation as a mystery when it was already explained in memory"
metadata:
  type: feedback
  originSessionId: 11522457-bd58-4bc7-b407-4880c5326371
  modified: 2026-09-11T19:53:35.544Z
---

Before flagging a data discrepancy as an unexplained bug/anomaly, check whether an existing memory already explains it — especially one written earlier in the same project.

**What happened:** Noticed Laser Auto Parts category `products_count` numbers (e.g. Audi root showing 2443 against a ~1812-product total catalog) and wrote it into memory as an unexplained platform anomaly needing investigation. The explanation was already sitting in [[project_laser_auto_parts_search_fix]] — every product on this store carries brand-root + part-type + model tags simultaneously, so a category's `products_count` (summed across its subtree) naturally exceeds the unique-product count. The user corrected this directly: "the category numbers are because many items are multitagged. u knew this u forgot."

**Why:** Treating something as a fresh mystery is more expensive than it looks — it invites a whole investigation task next session for a non-issue, and it signals to the user that earlier work isn't actually being retained/applied, which erodes trust in the memory system itself.

**How to apply:** When something looks surprising or "distorted," search existing project memory for related established facts (tagging behavior, known field bugs, prior data-shape notes) before writing a new "needs investigation" entry. This is especially important for numeric/count discrepancies on a store where multi-tagging, category rollups, or known tool bugs ([[project_laser_auto_parts_catalog_quality]] has 3 confirmed silently-broken fields) are already common explanations.
