---
name: feedback-checkpoint-before-stopping-agents
description: "Before stopping/killing a running background agent or fork, first ask it to persist its full accumulated progress (not just the last-processed item) to a resumable checkpoint file"
metadata: 
  node_type: memory
  pinned: false
  originSessionId: ec0727d0-4add-43d4-a582-4d5166861cbd
  modified: 2026-09-15T07:15:10.552Z
---

When the user asks to stop or "pause" a background agent/fork that is mid-task (e.g., a long paginated sweep), there is no real pause/resume for a subagent — `TaskStop` terminates it outright, and a later `SendMessage` to the same agent ID starts the work over rather than continuing from where it left off. The user corrected this directly after a duplicate-SKU-check fork was killed mid-sweep: "u should have saved progreess before pausing."

**How to apply going forward:** before calling `TaskStop` on any agent doing multi-step or long-running work, first send it a message asking it to write its complete accumulated state to a checkpoint file (not just whatever ad hoc partial-write file it happens to already have) — specifically, for a paginated/looping task, the checkpoint must include enough to resume cleanly: the current page/cursor position (or index reached) alongside the accumulated results so far, not merely the last individual item processed. Only call `TaskStop` after that checkpoint write is confirmed. If the situation requires stopping immediately with no time for a checkpoint round-trip, say so explicitly to the user rather than silently losing progress — a fork's own incidental temp-file writes (e.g., a buffer that gets overwritten every loop iteration) are not a substitute for an intentional, complete checkpoint and may only capture the most recent slice of work, not the full running total.
