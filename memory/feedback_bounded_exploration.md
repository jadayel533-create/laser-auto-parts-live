---
name: feedback-bounded-exploration
description: "User wants dashboard/UI exploration capped at a quick look, not exhaustive clicking — stop and ask when the answer isn't obvious"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 252b2f08-d83a-4fb3-b558-7a3dfd3f9e71
  modified: 2026-09-04T13:28:38.276Z
---

When exploring a dashboard or UI to find a feature/section (e.g. a Zid theme editor section type), cap it at a quick look. If the target isn't obviously present after that, stop and report what was found (e.g. list available section type names) and ask the user to choose, rather than continuing to scroll/click through every option.

**Why:** User explicitly asked for this bounded process on [[project_laser_auto_parts]] work (brand logo strip placement) to avoid wasted exploration time/tokens on repetitive dashboard UIs.
**How to apply:** For any similarly open-ended "find the right UI element/section" task, do 1-2 targeted checks max, then surface findings and ask rather than open-ended searching. Applies generally, not just to Zid.
