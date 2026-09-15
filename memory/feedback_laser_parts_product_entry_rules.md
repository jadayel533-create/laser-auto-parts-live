---
name: feedback-laser-parts-product-entry-rules
description: "Standing checklist for entering/editing any Laser Auto Parts product page — name/SKU/searchability, origin, fitment, image, stock, category, description quality, search verification"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: bd9e7098-a236-4415-97ef-572768b4d620
  modified: 2026-09-15T00:42:00.087Z
---

Owner set the original 5 rules 2026-09-13 (BMW gap-fill cleanup); owner restated/expanded into an 11-point checklist 2026-09-14 (catalog/SEO cleanup task). Treat the 2026-09-14 list as the current standing checklist — apply on EVERY future product touch on this store, not just the batch that triggered it.

**Full checklist (2026-09-14):**
1. Correct Arabic product name.
2. Correct OEM/reference number.
3. Both spaced and unspaced forms of the number included in searchable data (name/keywords/SKU-adjacent text) — so a customer searching either format finds it.
4. Manufacturer or factory-direct origin noted (not left blank/unknown).
5. Model, chassis, and years included.
6. Left/Right/Front/Rear position included when the part is side/position-specific.
7. Real (OE-confirmed) image before publishing — don't publish with a placeholder or no image.
8. Stock quantity and weight both set (not left at a placeholder like qty 0/1 or blank weight).
9. Category + model assignment (tag into every category tree the part genuinely fits, per [[feedback_multibrand_categorization]]).
10. Useful description written for the specific part, not mechanical/boilerplate template text (e.g. NOT generic "high-quality X, designed for reliable performance..." filler — write real, specific copy).
11. Search tested using the full number, the unspaced number, and a partial number — confirm the product is actually findable each way, don't just assume from data entry.
12. **Year range included in the product NAME/title itself** (e.g. "(2012-2018)"), not only in a description/compatibility table — the storefront's custom-JS Year filter ([[project_laser_auto_parts_search_fix]]) parses this pattern out of the product `name` field only; a product with real year data buried elsewhere is invisible to that filter. For a multi-brand/multi-model part with different per-brand ranges, still put one combined range in the title (widest span) even though the precise per-brand ranges live in the compatibility table.
13. **Manufacturer/supplier brand never appended to the SKU field** (e.g. no "/FEBI", "/LEM", "/TTC", "/SK" suffixes) — SKU must be the clean real OEM number only. Mention the manufacturer in the name/description text instead when known ([[feedback_laser_parts_manufacturer_naming_rule]]).

**Still-relevant detail from the original 2026-09-13 rules (mechanics, not superseded):**
- Name must be full: `<Part> [<Position>] for BMW <MODEL> <CHASSIS> (<years>) – OEM <number>` house convention, both languages.
- Strip a leading stray "C"/"T" SKU prefix on any product touched (known stock-file contamination pattern, see [[project_laser_auto_parts_catalog_quality]]).
- `Products.update`'s `sku` and `seo` params are confirmed no-ops via the API tool — SKU/SEO-title/SEO-description/URL-slug changes require the dashboard "Update existing products" bulk-import workaround (or direct per-product dashboard edit), never trust the API tool for these fields.
- Watch for bad literal/MT Arabic translations — use the established house dictionary in [[project_laser_auto_parts_catalog_quality]] instead of generating fresh translations (e.g. spark plug = بوجي/بواجي, ignition coil = كويل).
- Prioritize OEM part number/fitment over generic aftermarket cross-reference numbers when multiple identifications are possible.

**Why:** owner holds every product to a full completeness/quality bar, not just "does it exist" — batch-creation and batch-cleanup work (forks, bulk imports) tends to skip origin, weight, full-checklist description quality, and search verification unless explicitly told to check them each time.
**How to apply:** restate this full checklist (not just the mechanics) in any future product-creation or product-cleanup fork/task prompt for this store — don't assume it's inferred from general instructions.

**Escalated 2026-09-15, after items 12/13 above were found broken on live listings the owner had to catch by hand:** owner explicitly does not want to keep manually spot-checking published items for bugs like "missing year in title" or "manufacturer stuck in the SKU" — at the ~3,000-item scale this store is heading toward, manual catch-and-fix per item is not sustainable. **This checklist must be applied proactively at entry/publish time, in every creation and update path (direct API calls, forks, bulk-import files) — not treated as a follow-up QA pass the owner performs after the fact.** Before reporting any batch of product creates/updates as done, self-verify against this full checklist (a small live spot-check of the actual rendered page/filter behavor, not just trusting the API response shape) rather than waiting for the owner to find the gap.
