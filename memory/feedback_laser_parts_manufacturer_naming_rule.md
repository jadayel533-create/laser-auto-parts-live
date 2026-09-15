---
name: feedback-laser-parts-manufacturer-naming-rule
description: "Manufacturer/supplier brand name belongs in product name/description when known, never in the OEM/SKU field"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 8a553245-c90e-41bc-bcf2-9c6a68f0da79
  modified: 2026-09-15T00:33:51.531Z
---

Owner rule (2026-09-15): the manufacturer/supplier brand (e.g. FEBI, TRW, TTC, LEMFORDER, SK, TIBAO, BREMBO) should be mentioned in the product **name/description** wherever known and convenient — but must **never** appear inside the **OEM number / SKU** field.

**Why:** the SKU should stay a clean, real OEM part number — supplier-brand suffixes glued onto it (e.g. "8K0 407 505A/FEBI", "4H0 615 121J/SK") are data-entry artifacts, not part of the actual OEM code, and pollute SKU-based matching/search. The manufacturer is still useful information for the customer, just belongs in the readable name/description text instead.

**How to apply:** already consistent with the established SKU-cleaning convention ([[project_laser_auto_parts_catalog_quality]] — strip trailing "/BRANDNAME" before using as SKU). The new piece: when building a product's name or description going forward, include the manufacturer name in the text (e.g. "... — FEBI, OEM 8K0 407 505A" or a "Manufacturer: FEBI" line) whenever it's known from the source data, rather than dropping it entirely once stripped from the SKU. Applies to future creates/updates; retroactive fixes to already-created listings are a separate question — check with the owner before assuming that's wanted, since it could be a large scope depending on how many listings are affected.
