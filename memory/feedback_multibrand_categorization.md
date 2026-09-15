---
name: feedback-multibrand-categorization
description: "Laser Auto Parts Zid store - shared/cross-compatible parts must be tagged into every brand's category tree, not just their original brand"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 11522457-bd58-4bc7-b407-4880c5326371
  modified: 2026-09-11T05:13:57.741Z
---

When a product's title is corrected to mention multiple brands (e.g. a shared VW-Group part like G12E coolant that fits Porsche, Audi, and VW), also add it to the category tree for every brand named in the title — not just the brand it happened to be originally filed under.

**How to apply:** via `mcp__claude_ai_Zid__ProductCategories` `action:"add_product"`, tag the product into each additional brand's root category plus the matching part-type subcategory if one exists for that brand (e.g. Audi Cooling & AC id 1616298, Porsche Cooling & AC id 1616306). This is purely additive — never remove existing category tags. VW's category tree only has model subcategories (no part-type ones like "Cooling & AC"), so for VW just add the root (id 1697756).

**Why:** the user explicitly asked for this as standing procedure after a title fix (G12E 050 A2 coolant) revealed the product was only tagged under Porsche despite fitting Audi and VW too — without matching category tags, it wouldn't surface when a customer browses those other brands even though the title now says it fits them. Title accuracy and category placement need to stay in sync.

See [[project_laser_auto_parts]] for the store's full category ID reference and broader context.
