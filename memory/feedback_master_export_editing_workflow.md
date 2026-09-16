---
name: feedback-master-export-editing-workflow
description: "Laser Auto Parts standing workflow: maintain ONE running full-catalog export file that gets incrementally edited across sessions/brands, imported once at the end — not separate per-task Excel files"
metadata: 
  node_type: memory
  pinned: false
  originSessionId: ec0727d0-4add-43d4-a582-4d5166861cbd
  modified: 2026-09-15T09:05:33.173Z
---

The owner clarified this after a session lost track of which pending Excel file was "the" working file: **the established method is to take one fresh full-catalog export and keep editing it in place, accumulating fixes across multiple tasks and sessions, rather than building a new separate Excel file per task.** Their own words: "we took a fresh export and kept editing it like we are about to do now to import later after ironing existing parts."

**How this actually works, end to end:**
1. Pull one fresh full-catalog export (via `Products.export_to_email` or the dashboard's own export) as the single master working file.
2. Across however many sessions/tasks it takes, keep editing rows in that SAME file — cross-brand cleanup work like stray-letter-prefix SKU stripping, multi-model compatibility tables, and year fixes all accumulate into it rather than each spawning its own separate "import_ready_X.xlsx".
3. Meanwhile, brand-by-brand "ironing" work (fixing an individual live/published product's name, model, year, or image) happens via direct live API calls (`Products.update`, `ProductImages.add`, `ProductCategories.add_product`), not through this master file — that's a separate, faster path for one-off live fixes on already-published items.
4. Only once the live-stock "ironing" pass is done across all brands does the accumulated master export file get imported as one batch via the dashboard's bulk-import feature.

**Why this was confusing without this memory:** a file named for the first edit made to it (e.g. `import_ready_BMW_14groups_Porsche_Audi_update.xlsx`) can actually contain the full ~2,279-row catalog with edits from several unrelated tasks layered into it over time, not just the rows implied by its filename. **When resuming any Excel-based catalog cleanup task, don't assume a file's name describes its full scope — check the actual row count and content, and ask the owner which file is the current master if more than one candidate exists**, rather than treating a task-named file as freshly scoped to just that task.
