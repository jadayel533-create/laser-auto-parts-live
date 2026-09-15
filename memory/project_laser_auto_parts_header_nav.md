---
name: project-laser-auto-parts-header-nav
description: "Laser Auto Parts (Zid) — fixed the storefront header nav to show real brand dropdowns (BMW/Audi/Porsche/VW with model submenus) instead of generic 'All Categories', via the dashboard Main Menu builder, not custom JS"
metadata:
  type: project
  originSessionId: 252b2f08-d83a-4fb3-b558-7a3dfd3f9e71
  modified: 2026-09-13T19:29:47.637Z
---

Part of [[project_laser_auto_parts]]. Owner reported the header only showed generic "All Products"/"All Categories" links, no dropdown of the actual brands - fixed same day, 2026-09-13.

**Root cause: this was a dashboard configuration gap, not a custom-JS problem** - the header nav is controlled by Zid's own Main Menu builder (dashboard sidebar → المتجر الإلكتروني → القائمة الرئيسية, URL `/ar-sa/stores/3168409/channels/online-store/menu`), completely separate from the custom-scripts.js file this whole project has otherwise been built in.

**Two native Zid concepts that look similar but are NOT the same data:**
1. **"العلامات التجارية" (Brands)** - a toggle in the Main Menu builder that links to `/brands`, a totally different Zid entity from Categories (a product-level "Brand" attribute field). **Confirmed broken for this store: only ONE of the 4 brands (Volkswagen) has ever had this field populated** - turning this toggle on shows a `/brands` page with just a single Volkswagen card, which would have been MORE confusing than the original problem. **Turned back off. Don't re-enable this without first populating the Brand field on all BMW/Audi/Porsche products via the dashboard/API - not attempted, not clear if worth the effort given Custom Elements already solves the real need.**
2. **"العناصر المخصصة" (Custom Elements)** - the actual fix. A menu-item builder where you pick a link type (Product / Category / Legal Page / Additional Page / External Link) and a specific target. **Selecting "تصنيف" (Category) and picking a top-level brand category (e.g. "قطع غيار BMW") auto-populates the ENTIRE live subcategory tree as a dropdown under that item** - no manual per-model entry needed. Confirmed live: adding just "BMW" → category 1602249 auto-generated a dropdown with all 11 BMW model/series subcategories.

**How to apply (for adding/editing a brand's nav dropdown in future):** Main Menu page → scroll to "العناصر المخصصة" section → "إضافة رابط جديد" → fill Arabic + English label → set "نوع الرابط" to "تصنيف" → search/select the brand's top-level category → confirm. The subcategory dropdown appears automatically, no further steps.

**Propagation delay observed: dashboard save is instant (toast confirms "تم تحديث القائمة بنجاح") but the LIVE storefront header did not reflect the change for several minutes even with cache-busting query params on repeated fresh navigations.** Not fully explained (CDN/edge cache of rendered nav component, most likely) - don't assume a save failed just because a fresh page load doesn't show it immediately; wait a few minutes and recheck before troubleshooting further. This cost real back-and-forth this session before it finally went live.

**Session division of labor:** Claude built the BMW dropdown (verified the mechanism, added BMW, turned off the broken Brands toggle) while the owner independently added Audi/Porsche/Volkswagen the same way in the dashboard UI once shown how - both landed live around the same time. **Final live header confirmed via screenshot:** clean dropdown nav showing "BMW", "قطع اودي" (Audi), "قطع بورش" (Porsche), "قطع فولكس واجن" (VW), plus "المزيد" (More) for the rest, each with full model/part-type submenus.
