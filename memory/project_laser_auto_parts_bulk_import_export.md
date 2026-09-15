---
name: project-laser-auto-parts-bulk-import-export
description: "Laser Auto Parts (Zid) — the real fix for bulk-editing 2000+ product names/descriptions and adding un-entered stock is Zid's native Import/Export feature (Products page overflow menu), not custom code or extra AI-tool permissions"
metadata:
  type: project
  originSessionId: 252b2f08-d83a-4fb3-b558-7a3dfd3f9e71
  modified: 2026-09-13T19:34:03.552Z
---

Part of [[project_laser_auto_parts]]. Owner's real need: with 2000+ products (heading to 3k), many titles/descriptions need polishing and some stock was never entered - said "not manageble without bulk edit" and initially framed this as needing broader AI/desktop permissions. **The actual fix needed no extra permissions at all - it's a native dashboard feature.**

**Where it lives:** Dashboard → Products page → the "⋮" (overflow) menu. Contains:
- **"Export All"** - downloads the full catalog as an Excel file.
- **"Import Products"** - 4 distinct modes, easy to pick the wrong one by accident:
  1. import-new (adds rows as new products, does not touch existing ones)
  2. update-existing-by-SKU (matches on SKU, overwrites fields) - **this is the one for the owner's "polish names/descriptions and add missing stock" goal**
  3. delete-and-replace-all (destructive - wipes and reloads the whole catalog from the file)
  4. update-quantities-only (narrow, stock-only)
- Zid provides a downloadable Excel **template** with the exact expected columns for import.

**Recommended workflow for the owner's actual goal:** Export All → edit names_ar/name_en/description fields and fill in missing quantity/stock cells directly in Excel → re-import using **update-existing-by-SKU** mode (never delete-and-replace-all, which is a destructive full-catalog wipe+reload and should not be used for a routine edit pass).

**Template columns observed** (from `import_products_example_2026.xlsx`, ~114 columns total): sku, name_ar, name_en, price, quantity, categories_ar/en, description_ar/en, short_description_ar/en, images, keywords, barcode, plus option/variant fields and filtration fields. Full column list was extracted this session by unzipping the xlsx and parsing `xl/sharedStrings.xml` - re-derive the same way if the exact column set is needed again, since Zid may revise the template over time.

**Why this matters going forward:** any future "I need to bulk-change lots of products" request from the owner should default to this native Import/Export path first, not a custom script or dashboard-automation approach - it's already built, free, and handles the owner's real scale (2000-3000+ SKUs) natively.
