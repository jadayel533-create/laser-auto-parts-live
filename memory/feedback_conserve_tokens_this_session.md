---
name: feedback-conserve-tokens-this-session
description: User asked to be conservative with tokens after hitting the session rate limit 3x in one day on the Laser Auto Parts catalog-expansion project
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 8a553245-c90e-41bc-bcf2-9c6a68f0da79
  modified: 2026-09-14T23:39:21.195Z
---

User said "this session u have to be conservative with tokens" (2026-09-15), right after the session had already hit its rate limit 3 times in one day (see [[project_laser_auto_parts_catalog_quality]] — the per-item live-API create/categorize/image-upload pattern was the cause, since replaced with an Excel-bulk-import pipeline).

**Why:** repeated session-limit hits from token-heavy per-item API loops cost real time and disrupted the work; the user wants leaner execution going forward, not just for this specific bug but as a general operating mode for the rest of the session.

**How to apply:** favor the cheaper mechanism whenever one exists (e.g. bulk file + one import action over many individual API calls), avoid redundant verification/screenshot loops, keep background-agent batch sizes and counts modest, don't re-check things already confirmed, and keep responses/tool use lean rather than exhaustive. Still verify real results before reporting success (don't skip correctness checks entirely) — the ask is efficiency, not carelessness. Re-check whether this preference is still in force in future sessions rather than assuming it's permanent, since it was stated as scoped to "this session."
