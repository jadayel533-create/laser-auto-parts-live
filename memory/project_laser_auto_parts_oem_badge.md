---
name: project-laser-auto-parts-oem-badge
description: "Laser Auto Parts — 'OEM Verified' trust badge on product cards/detail pages, deployed via custom-scripts.js. Live and verified across all 4 brands as of 2026-09-14."
metadata: 
  node_type: memory
  type: project
  originSessionId: bd9e7098-a236-4415-97ef-572768b4d620
  modified: 2026-09-13T21:05:35.357Z
---

Part of [[project_laser_auto_parts]]. Implemented following the competitive-benchmark report's recommendation (see the "Laser Auto Parts Benchmark" artifact) — a low-cost trust signal since product reviews are still disabled and this reinforces the store's genuine OEM-number-verified angle.

**Badge copy:** English "OEM Verified" · Arabic "رقم القطعة موثق" (owner's explicit wording — not a literal translation, corrected once already from an initial "رقم قطعة أصلي موثّق").

**Mechanism:** same custom-scripts.js injection pattern as every other storefront feature on this store (dark mode, footer links, English-name display, VIN search) — deployed via the "الثيمات" page's ⋮ → تعديل JS editor, append-only. Detects an OEM number already present in the product's title/description text and injects the badge on both product cards (category/listing pages) and the product detail page; no per-product API calls needed, pure client-side text detection off already-rendered content.

**Bugs hit and fixed during this implementation, useful if this pattern is reused elsewhere on this store:**
1. `execCommand('insertText')` silently no-op'd on the first attempt; a `view.dispatch()` retry worked, but doing both left duplicated code in the file — caught before saving. **If `insertText` seems to do nothing, verify doc length actually changed before retrying with `dispatch()`, or you'll double-insert.**
2. A "mark card as checked" guard was set before the card's text had fully hydrated, permanently skipping ~80% of real matches (guard fired once on the loading/empty state and never re-checked). **Fixed by guarding on "badge already present" instead of "card already visited" — always re-derive the skip condition from the current DOM state, not a one-time visited flag, when content loads asynchronously.**
3. **Root cause of the "BMW-only" bug:** the detection regex only matched bare numeric OEM strings (BMW's SKU format, e.g. `51137295356`). Audi/Porsche/VW use spaced alphanumeric VAG-style part numbers (e.g. `4L0 809 962B`) which didn't match. Broadened to accept alphanumeric+space tokens containing at least one digit. **Any future regex work on this store's OEM/part-number text must account for both formats — BMW bare-numeric vs. VAG alphanumeric-with-spaces — never assume one pattern covers all 4 brands.**
4. Card detection scanned title+description, but detail-page detection only scanned the title — caused a real inconsistency (badged on the category card, not on its own detail page for the same product). Unified both to scan the same full text block.

**Verified live 2026-09-14 across all 4 brands (BMW, Audi, Porsche, VW):** badge correctly appears where a genuine OEM number is present, correctly absent on generic/bulk parts with no OEM callout in their text. Checked light + dark mode and desktop + mobile (390px, Playwright) — clean RTL layout, good contrast both themes, no overflow.
