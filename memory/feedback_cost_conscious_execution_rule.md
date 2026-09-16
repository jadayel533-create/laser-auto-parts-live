---
name: feedback-cost-conscious-execution-rule
description: "Standing rule to conserve usage: no large/bulk API call batches or forks without asking first, and prefer a local-Excel search/edit/import workflow over repeated live API queries"
metadata: 
  node_type: memory
  pinned: true
  originSessionId: ec0727d0-4add-43d4-a582-4d5166861cbd
  modified: 2026-09-15T07:29:50.526Z
---

The owner set this as an explicit standing rule after a background fork doing a full 46-page catalog pagination sweep (to check ~100+ SKUs for duplicates) burned 40% of a 5-hour session usage window, on top of several earlier sessions where usage limits were hit repeatedly. Their own words: "write down a rule no going wild with api calls without permission simple tasks are fine. also no more going wild with forks. plan reasonably to save cost. always excel search in it edit in it then import. we need to be saving usage."

Apply this on every task, not just the Laser Auto Parts project:

1. **Don't run large/bulk batches of API calls without asking first.** A single call or a handful of calls for a simple, bounded task is fine and doesn't need permission. But anything that amounts to a sweep or grind — paginating an entire catalog/dataset, looping per-item calls across dozens+ of records, or any plan whose cost is hard to bound in advance — needs a quick check-in with the owner before starting, not just before continuing once it's already expensive.
2. **Don't spawn forks/subagents liberally.** Each fork has its own real usage cost (confirmed: two forks on one sub-task alone cost hundreds of thousands of tokens, much of it wasted on the fork re-exploring a method the coordinating session already understood). Only fork when the task is well-scoped, mechanical, and the delegation itself is worth the overhead — and check with the owner first for anything non-trivial, rather than defaulting to delegation.
3. **Prefer a local-file (Excel) workflow over live API round-trips for search/verification work.** Pull the relevant data into a local file once (e.g., via a single bulk/paged export or existing local data), then do searching, filtering, deduping, and editing against that local file with scripts — not by repeatedly querying the live store API. Only go back to the live API/dashboard for the final import step.
4. **When in doubt about cost, ask before executing, not after.** The owner is on a usage-constrained plan and has hit weekly/session limits multiple times; treat usage as a real, scarce resource on every task, not just when explicitly reminded.
