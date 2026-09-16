---
name: project-laser-auto-parts-audi-compatibility-tables
description: Audi/BMW fitment-completeness project — compatibility tables live for 11 duplicate-OEM listings; sample-50 audit shows ~64% of Audi catalog likely under-states real fitment; BMW seed uploaded, unstarted
metadata: 
  node_type: memory
  type: project
  modified: 2026-09-15T17:24:48.708Z
  originSessionId: 8a553245-c90e-41bc-bcf2-9c6a68f0da79
---

Part of [[project_laser_auto_parts_catalog_quality]]. Owner uploaded `Laser_Audi_Fitment_Seed1.xlsx` (892-row audit of the existing Audi catalog's fitment-data completeness — 465 "ready for verification", 169 missing model, 246 missing year/range, 12 missing both, 41 OEMs appear on multiple listings across the whole catalog but only 5 of those are both live/published, 249 have images). Full seed file, not yet acted on beyond the scope below.

**Piloted and then rolled out 2026-09-15: bilingual HTML compatibility tables on product pages**, for the 5 OEM groups that were both (a) live/published and (b) had multiple listings in the seed data — 11 listings total. For each: verified real multi-model/multi-brand fitment via WebSearch (not just trusting the single model the original listing claimed), expanded the product **name** to list every confirmed model, appended a compatibility table (with per-model years) to the **description** field, and tagged every matching category via `ProductCategories.add_product` — including genuine cross-brand tagging (`7L0407183A` fits Audi Q7 + VW Touareg + Porsche Cayenne, now tagged into all three brand trees).
- Table renders cleanly on the storefront under "المميزات والتفاصيل" → confirmed live on the pilot item.
- One stale data error caught and corrected: `8K0 498 203B/FEBI` had a wrong "2002-2006" year baked into its original title (real range is 2008-2017 for that OEM); noted in the corrected description.
- The 5 groups / OEMs: `8K0407505A` (control arm upper front — A4/A5/A6/A7/Q5), `8K0498203B` (CV boot — A4/A5/Q5, manual-trans only), `4G0407183B` (control arm bushing — A6/A7), `4H0615121J` (brake pad wear sensor — A6/A7/A8/Q5), `7L0407183A` (control arm bushing — Audi Q7/VW Touareg/Porsche Cayenne).

**Known incomplete, flagged by owner 2026-09-15, not yet fixed — do this before considering the rollout done:** `short_description` (ar/en) and `seo.title`/`seo.description` (ar/en) were NOT updated on any of the 11 listings — they still show the original single-model text, and on `8K0 498 203B/FEBI` specifically still cite the wrong "2002-2006" year. The main `name` and `description` fields are correct; these secondary fields are stale until a follow-up pass touches them.

**REAL BUG found and FIXED live 2026-09-15: the two Group-5 (`7L0407183A`, Q7/Touareg/Cayenne) listings had NO year at all in their product name/title** (title was just `"Front Control Arm Bushing - Audi Q7 / VW Touareg / Porsche Cayenne"` — the per-brand years lived only in the compatibility table, not the title, because the three brands' real ranges differ). The storefront's custom-JS Year filter ([[project_laser_auto_parts_search_fix]]) parses a `(YYYY)`/`(YYYY-YYYY)` pattern out of the product **name** field only — a product with real year data anywhere else is invisible to that filter on every category page it's tagged into. Owner caught this live with a concrete repro ("7L0 407 183A/LEM doesn't show up when filtering touareg 2004").
**Fix:** added the combined widest range to both listings' titles: `"... (2002-2018)"` (VW Touareg's range, which is the widest of the three — Q7 is narrower at 2007-2015, Cayenne narrower still at 2003-2010; the per-brand precision stays in the compatibility table, the title just needs *a* parseable range for the filter to index it at all). **Verified live:** VW Touareg category page, Year filter set to 2004 (dispatched a real `change` event on the `<select>`, not just `.value=`), confirmed the LEM listing's brand name text appears in the filtered results.
**Lesson generalized into the standing checklist:** [[feedback_laser_parts_product_entry_rules]] point 12 — any future product touch must have a year range embedded in the NAME field itself, not just in a description table, especially for cross-brand/multi-model parts where it's tempting to leave the title year-free since no single range is fully precise.
**Groups 1-4 were NOT affected** (their titles already ended in a shared single-brand year range, e.g. "Audi A4/A5/A6/A7/Q5 (2008-2018)") — only Group 5's cross-brand pair had zero year in the title.

**Owner pushback 2026-09-15 on the "11 items" framing — correctly identified as a severe undercount, not a real audit:** the 5 groups / 11 listings were found via ONE cheap proxy signal (OEMs that already had duplicate listings in the catalog) — NOT a measurement of how many Audi parts genuinely have multi-model fitment. The pilot item alone (`8K0407505A`) turned out to fit ~10 model/trim/year combinations from a single WebSearch, and this is typical for VAG-platform parts (Audi/VW/Porsche/Seat/Skoda share suspension/brake/engine components across generations constantly) — most of the other ~880 single-listing Audi products almost certainly have similarly broad real fitment that's never been checked, since "already has a duplicate listing" only catches a small, essentially arbitrary subset.
**Sample-50 measurement started to quantify the real hit-rate before committing to a full-catalog audit:** input `C:\Users\user\AppData\Local\Temp\claude\xlsxtool\sample50_audi.json` (50 published Audi listings, evenly spread across the whole 256-published-item set via a stride sample, not just the first 50) — for each, WebSearch the OEM's real independently-sourced fitment vs. what's currently listed. Output `C:\Users\user\AppData\Local\Temp\claude\xlsxtool\sample50_results.json`.
**KILLED by owner mid-run 2026-09-15 at 25/50 — but the partial data is still a real, usable signal, not discarded:** of the 25 completed, **16 (64%) came back `broader_than_listed: true`** — i.e. real fitment genuinely wider than the single model+year currently listed — vs. 9 confirmed already-accurate as single-model. **This strongly confirms the owner's skepticism: the true rate of "this Audi listing under-states its real fitment" is roughly two-thirds, not the ~1.2% (11/892) the duplicate-listing proxy implied.** If resuming this thread: re-run the same sample (or a fresh one) to completion, or treat 64% as directionally solid enough to greenlight a real full-catalog fitment audit — that decision was not made before the owner paused to consolidate memory (see the "wrap up for a different account" context below).

**Separately uploaded, NOT yet started: `Laser_BMW_Fitment_Seed.xlsx`** — same audit structure as the Audi seed file, for BMW: 856 total listings, 387 published (355 published+fitment-complete), 30 missing model/series, 15 missing both year and chassis, 48 identifiers with multiple listings (same narrow duplicate-listing proxy, same caveat as Audi's 41 applies), 580/856 have images. **Structural difference worth reusing:** this file already ships a separate "SKU suffix" column (e.g. `LENS`, `AFTERMARKET`) pre-split from the base SKU — potentially useful input for the still-unstarted [[project_laser_auto_parts_catalog_quality]] SKU manufacturer-suffix cleanup task. Natural next step once/if the Audi multi-model audit approach is validated: same method, BMW catalog.

**Source file location caveat:** both seed files (`Laser_Audi_Fitment_Seed1.xlsx`, `Laser_BMW_Fitment_Seed.xlsx`) live under this session's own upload folder (`C:\Users\user\.claude\uploads\8a553245-c90e-41bc-bcf2-9c6a68f0da79\...`) — a session-scoped path. **A future session (especially from a different account) will likely NOT be able to read these files directly** and may need the owner to re-share them. The derived working files in `C:\Users\user\AppData\Local\Temp\claude\xlsxtool\` (`sample50_audi.json`, `sample50_results.json`, the merged Audi 892-row data if re-extracted) are separately at risk of temp-directory cleanup — this memory file's own prose captures the essential numbers/findings so nothing is lost even if both source locations become unreachable.

**Session wrap note 2026-09-15 (owner may continue this project from a different account):** everything above is the true current state — nothing beyond the 11 listings' name/description/category fields has been changed; short_description/SEO on those 11 is still stale; the Audi full-catalog audit decision is unmade; the BMW seed file is unexamined beyond the Summary-sheet stats recorded here. Read this file plus [[project_laser_auto_parts_catalog_quality]] fully before resuming — don't re-derive anything already established here.

**Scope note:** this only covers the 5 live duplicate-OEM groups (11 listings). The seed file's other ~830 rows (single-listing items, hidden duplicates, missing-model/missing-year items) are unexamined — owner scoped this explicitly to "live products only" and the duplicate-OEM subset as the first/cheapest win, not a full rollout. If asked to expand scope later, re-read the full seed file rather than re-deriving the audit.

**2026-09-15 session: all 4 brand seeds now exist** (`Laser_Audi_Fitment_Seed1.xlsx`, `Laser_BMW_Fitment_Seed.xlsx`, `Laser_Volkswagen_Fitment_Seed.xlsx`, `Laser_Porsche_Cayenne_Panamera_Fitment_Seed1.xlsx`). **VW seed stats:** 111 VW-relevant listings, 13 published, 7 published+fitment-complete, 20 missing a specific model (generic "VW" only), only 3 identifiers with multiple listings (much lower duplicate-rate than Audi/BMW). **Porsche seed stats** (Cayenne/Panamera only): 127 listings, 7 published, 7 published+ready, only 2 identifiers with multiple listings and both are unpublished SKU-suffix duplicates (no genuine within-brand multi-model case in Porsche's own catalog). All 4 seed files live under this session's own upload folder (`C:\Users\user\.claude\uploads\3cec27c5-4682-4a99-bb70-eab9668c34d7\...`), same session-scoped-path caveat as the Audi/BMW ones below — re-derive rather than expect to re-read them from a different session. Cross-referencing all 4 for genuine multi-model candidates (same test as above: does one duplicate listing state broader fitment than its sibling for the same OEM, not just "has duplicates" — most BMW duplicates are literally the same fitment restated by a different supplier, no new info) found:
- **83220144137** (6HP ATF fluid, BMW/Audi/VW/Porsche) — **split state, read carefully:**
  - **ZF variant (SKU `83 22 0 144 137Z`) is LIVE and verified** — this was done *before* the Chrome-click ban below existed, via direct dashboard click-editing (the ~40-minute edit that prompted the ban in the first place). Name (EN+AR) updated to all 4 brands, categories tagged (BMW/Audi/VW general + BMW 3rd/5th Series + Porsche general/Cayenne + Audi general), Arabic description got a BMW/Audi/VW/Porsche compatibility table. Confirmed persisted after a hard page reload.
  - **Febi variant (SKU `83 22 0 144 137F`) is NOT live** — prepared the correct way (Excel export → edit cells → reparse-verify), matching content to the ZF sibling (own opening sentence kept, same compatibility table appended, same 4-brand categories, English description left as originally written to mirror what was done for ZF). File: `import_ready_83220144137_Febi_update.xlsx` in Downloads. **Still sitting unimported** — owner has not pulled the trigger on any import yet this session.
- **14 BMW multi-model groups** where one listing under-states fitment vs. its sibling (e.g. a brake pad sensor listed as "1/3 Series" on one row, "3/4 Series" on the duplicate) — fully researched, Excel update built (`import_ready_BMW_14groups_Porsche_Audi_update.xlsx` in Downloads, 32 SKU rows), **not yet imported**, owner reviewing first.
- **G 555 502 M2** (Audi/VW 5W-40 engine oil) — already tagged into all 3 brands' categories including Porsche from a prior session; only needed a name/description fix (name said "Audi, Volkswagen" only, description was generic boilerplate) — included in the same update file.
- **83222365987** (BMW/Audi/Bentley differential oil) — name/description already accurate, just needed the Audi category tag added — included in the same update file.
- **83212365930 (real bug caught): NOT a genuine multi-supplier duplicate.** Three listings (5W-40/10W-40/TwinPower-5W30-LL01) share one OEM only because the seed's normalization stripped a real distinguishing suffix, not a supplier code — confirmed via the FAPI cross-reference API (see below): the bare OEM is specifically "TwinPower Turbo 5W-30 LL01," cross-referenced only with other 5W-30 oils, not 5W-40/10W-40 products. **Excluded from all compatibility-table work — flagged for a separate SKU/data-correction pass, not touched.**
- No new candidates in the VW or Porsche seeds beyond what's above (VW's only duplicate is 83220144137; Porsche has zero within-brand duplicates).

**FAPI Catalog API discovered/adopted this session** — a genuine OEM cross-reference + vehicle-applicability database at `https://fapi.iisis.ru/fapi/v2` (docs: fapi-dev.github.io/catalog-openapi), auth via `?ui=<key>` query param. A public demo key is published at a gist linked from the docs (rotates periodically). Endpoints that worked on the demo key: `/manufacturerList` (brand name → `dbi`, e.g. BMW=11659), `/productList?n=<oem>` (find catalog entries for a part number across brands), `/analogList?n=<oem>&mfi=<dbi>` (the cross-reference graph — this is what caught the 83212365930 error and confirmed all 14 BMW groups are genuine single parts). `/productApplicabilityList` (vehicle make/model/modification for a part) returned empty even for the docs' own example on the demo key — likely gated to a paid tier; do not rely on it without testing again with a permanent key. **This is now the preferred verification method over generic WebSearch** for "is this really the same part" questions — faster, structured, and caught a real data error WebSearch-style spot-checking had not (WebSearch spot-checks on other BMW OEMs did also confirm correctly, so both methods agree when the seed data is clean — FAPI is just more reliable for catching cases where it isn't).

**Standing rule from this session, see [[feedback_zid_product_edit_method_rule]]:** all of this work must go through Zid's Excel Import/Export (export → edit cells → re-import by SKU), never Chrome click-editing of individual products — that memory has the full rule and the reasoning (a single click-edited product took ~40 minutes with several near-miss data corruptions from a finicky rich-text table editor).

**SESSION WRAP 2026-09-15 (later session, replaces the wrap note below as the current resume point) — owner expanded scope to a full live-catalog cleanup and asked for a ready-to-paste prompt for the next session.** Owner's own framing: the manufacturer-never-in-SKU rule (see [[feedback_laser_parts_manufacturer_naming_rule]]) was written AFTER most of the catalog was already created, so assume the SKU-purity problem is catalog-wide, not limited to the 166 `/SUPPLIER`-suffix rows already found — some products may have a manufacturer name baked into the SKU without a `/` delimiter at all, or other non-OEM text. This session also found and fixed a live data bug: 2 Porsche products (`970 331 043 00`, `970 331 047 00`) had a wrong bushing photo uploaded to them by an unknown concurrent process mid-session — always re-check `ProductImages.list` immediately before any upload even on a freshly-swept "no image" list.

---

## READY-TO-PASTE PROMPT FOR NEXT SESSION

```
Laser Auto Parts (Zid store 3168409) — full live-catalog cleanup pass. Read
[[project_laser_auto_parts_catalog_quality]] and [[project_laser_auto_parts_audi_compatibility_tables]]
in full before starting; don't re-derive anything already established there
(tool bugs, the dashboard bulk-import mechanism, image-sourcing method, the
established Arabic parts-name dictionary, cost-conscious execution rules).

STEP 0 — ALWAYS START WITH A FRESH EXPORT. Do not trust any locally-cached
xlsx/json from a prior session as current truth — the live catalog changes
between sessions (confirmed multiple times: concurrent edits, stale gap
lists). Pull a brand-new full-catalog export via the dashboard (Products →
⋮ → تصدير الكل → تصدير جميع تفاصيل المنتجات, or Products.export_to_email)
before auditing anything. Reference the 4 existing fitment seed files as a
starting point/shortlist, not as ground truth:
  - Audi:    C:\Users\user\.claude\uploads\8a553245-c90e-41bc-bcf2-9c6a68f0da79\92df8154-Laser_Audi_Fitment_Seed1.xlsx (sheet "Audi Fitment", 892 rows, header row is row 3 — rows 1-2 are a title/note)
  - BMW:     C:\Users\user\.claude\uploads\8a553245-c90e-41bc-bcf2-9c6a68f0da79\a7586ef6-Laser_BMW_Fitment_Seed.xlsx (sheet "BMW Fitment", 856 rows, same header-row-3 layout)
  - VW:      C:\Users\user\.claude\uploads\3cec27c5-4682-4a99-bb70-eab9668c34d7\06a4a439-Laser_Volkswagen_Fitment_Seed.xlsx (sheet "VW Fitment", 111 rows)
  - Porsche: C:\Users\user\.claude\uploads\3cec27c5-4682-4a99-bb70-eab9668c34d7\77dd8348-Laser_Porsche_Cayenne_Panamera_Fitment_Seed1.xlsx (sheet "Porsche Fitment", 127 rows, Cayenne/Panamera only)
  These session-scoped upload paths may be unreadable from a different
  account/session — ask the owner to re-share if so. Each seed already has
  a pre-split "SKU suffix"/"Supplier suffix" column — reuse it rather than
  re-parsing. All 4 seeds explicitly say fitment has NOT been independently
  verified — treat the model/year columns as a lead to check, not fact.

Apply these four fixes to every LIVE (published) product, brand by brand:

1. SKU PURITY — every live SKU must be a clean, real OEM number only, no
   manufacturer/supplier text anywhere in it (not just the already-known
   `/SUPPLIER` trailing pattern — also check for a brand name glued on with
   no delimiter, or embedded mid-string). Known brand tokens to scan for:
   FEBI, TRW, TTC, LEMFORDER/LEM, SK, TIBAO, BREMBO, MANN, MAHLE, HENGST,
   BOSCH, NGK, ZF, VALEO, DELPHI, NRF, HEPU, NISSENS, RIDEX, MEYLE, SWAG,
   TOPRAN, VAICO, METZGER, BLIC, PRASCO, DPA, JUMASA, KLOKKERHOLM, LENS,
   AUTO, CHINA, BLSTN/BILSTEIN, AST, TXT/TEXTAR, BAIER, WINGS, FREY, GST,
   NISSENE, TRC, SCH, ELRING, BRYMAN, BUSH — this list is not exhaustive,
   pattern-match generally. If two products share one true OEM because
   they're genuinely distinct supplier stock lines (real case, not a data
   error — confirmed via the original STOCK_2026.xls or a fresh check),
   keep them as separate products and give each a short unique suffix on
   the SKU itself (this store's own established precedent: `83220144137Z`
   vs `83220144137F` for ZF vs Febi) — never a full brand-name word glued
   on. Move the full manufacturer name into the product name/description
   wherever known, per [[feedback_laser_parts_manufacturer_naming_rule]].
   Fix mechanism: dashboard bulk-import "update existing products", set
   `new_sku`, matching by current `sku` — same mechanism already used for
   the 843-item leading-"C "-prefix cleanup. Pre-check every candidate
   `new_sku` against a fresh full export before submitting — one collision
   fails the entire batch atomically with no useful UI error (see
   [[project_laser_auto_parts_catalog_quality]] for how to read the real
   error via a fetch-wrapper injection).

2. MODEL COMPLETENESS — any live item with no model in its name/category
   gets one added, verified via WebSearch/AutoDoc/FAPI catalog API
   (https://fapi.iisis.ru/fapi/v2, see this file's own notes above for
   endpoints), never guessed and never left as the stock file's own
   unreliable shorthand text.

3. MULTI-MODEL ITEMS — for any item confirmed to genuinely fit more than
   one model (very common for VAG-platform and BMW-platform shared parts —
   the sample-50 Audi audit below found ~64% of Audi listings understate
   real fitment), write the FULL model list into both the name field AND
   the description, and add a bilingual HTML compatibility table (same
   format as the 5-group pilot documented above in this file) with
   per-model year ranges. The title must still carry at least one
   parseable `(YYYY)`/`(YYYY-YYYY)` range even for cross-brand items whose
   per-brand ranges differ (use the widest range) — the storefront's Year
   filter parses only the name field; a title with no year is invisible to
   it even if the compatibility table has real year data (real bug found
   and fixed live 2026-09-15, see above).

4. CATEGORY TAGGING — every item must be tagged (`ProductCategories.add_
   product` or the bulk-import `categories_ar`/`categories_en` columns)
   into EVERY subcategory it genuinely matches — root brand + every
   matching model subcategory + every matching cross-brand tree for
   genuinely shared parts, not just its originally-assigned one.

5. SINGLE-YEAR FIX — any item whose title shows only one bare year (not a
   range) gets corrected to the real full year range for that OEM, verified
   externally (WebSearch/AutoDoc/FAPI), never left as a suspicious lone
   year (owner's own observation: "many products have only one year
   listed, which can't be true").

**SKU-SUFFIX FIX PUSHED LIVE 2026-09-15 (same session as the discovery above) — 307 of the 458 seed-identified suffix SKUs fixed and confirmed live.** Built the full fix list from all 4 seed files' pre-computed "OEM normalized"/suffix columns (458 total candidates: Audi 95, BMW 349, VW 10, Porsche 4), cross-checked every one against a fresh `master_export_working.xlsx` snapshot, and split into two groups:
- **307 safe (zero collision) — imported via dashboard "update existing products", SKU field only, `new_sku` = clean OEM, "ignore empty fields" on.** Confirmed live via direct dashboard search on 2 spot-checks (`4G0853651`, `97010615103`, both showing "updated 8 minutes ago"); total catalog count stayed at 2,278 (no duplicates created). File: `C:\Users\user\AppData\Local\Temp\claude\xlsxtool\sku_suffix_fix_safe_import.xlsx`. First import attempt returned a console-only 400 (`POST /v2/products/management/import`), second click on the same file succeeded (201) — if this recurs, retry once before investigating further, the UI gives no visible error either way.
- **151 held back, NOT pushed, need owner review before any action** — saved to `sku_fix_needs_review.json` in the same folder: 84 are internal collisions (two different current SKUs both normalize to the same target — same "genuine multi-supplier duplicate" pattern as the 6 groups in the 115-item new-product batch, need a unique disambiguating suffix each, not a shared clean SKU), 50 are live collisions (the clean target SKU already exists as a SEPARATE live product — near-certainly genuine duplicate listings at scale, same pattern as the already-documented BMW fender LH/RH pair, needs the owner to decide which to keep/merge for each), 17 are both. **Do not blindly apply the same fix mechanism to these 151** — a fresh per-group decision is needed first, exactly like the 6 groups already resolved in the 115-item batch discussion above.
- Reusable script: `C:\Users\user\AppData\Local\Temp\claude\xlsxtool\build_full_suffix_fix.js` (regenerates the full 458-row candidate list from the seeds) and `split_safe_vs_review.js` (splits by collision status against a fresh master export) — rerun both against a fresh export before doing anything with the 151, don't trust this session's split as still current.

**SECOND PUSH same session, 138 more fixed — owner supplied the disambiguation rule for the 151 held-back items: "if price is different then they're a different manufacturer."** Applied it by pulling each SKU's actual current `price` from `master_export_working.xlsx` and grouping the 151 by target OEM: **87 groups (138 individual SKU rows) had genuinely different prices per item — confirmed distinct manufacturer/supplier lines**, gave each a unique short-letter suffix on the clean OEM (e.g. `8K0 407 505A/FEBI`→`8K0407505AF`, `8K0 407 505A/TTC`→`8K0407505AT`, matching the store's own existing `83220144137Z`/`83220144137F` precedent — first letter of the original supplier tag, extended to 2+ letters only if that collides within the same group). **10 groups had identical prices across their items — these were NOT touched, still flagged as likely genuine duplicate listings needing manual merge/keep-one decision** (saved in `review_same_price.json`): `7L5907637B`, `4H0407151B`, `4H0407152B`, `8R0615121`, `1K0121251AB`, `80A853651`, `12131712219`, `31316789363`, `34116764540`, `11428637821`.
  - Verified zero real collisions before pushing (a first collision-check pass falsely flagged 66 rows as "colliding with themselves" — bug was comparing normalized new-SKU against the full catalog including the row's own current SKU; fixed by excluding self-matches, then confirmed all 138 raw SKU strings differ from their current value and don't collide with any other product).
  - Pushed via the same dashboard mechanism (update existing products, SKU field only, `new_sku` set). File: `sku_distinct_manufacturer_import.xlsx`. Import call returned 201 on the first click this time (the first push's 400-then-201 pattern didn't recur). **Spot-check note: the dashboard's own exact-SKU search has a transient index lag right after an import — searching the new SKU directly returned zero results immediately after, but a broader partial-text search (e.g. `407 505A` instead of the full new SKU) found all 3 sibling rows correctly with fresh "updated N minutes ago" timestamps.** Don't mistake that lag for a failed import — verify with a partial/broader search, not just the exact new value, if checking immediately after a push.
  - Catalog count held at 2,278 through both pushes — total live SKU-suffix fixes this session: 307 + 138 = 445 of the 458 seed-identified candidates. Remaining 13 (the 10 same-price groups below) still need the owner's manual review, not yet touched:
    `7L5907637B`, `4H0407151B`, `4H0407152B`, `8R0615121`, `1K0121251AB`, `80A853651`, `12131712219`, `31316789363`, `34116764540`, `11428637821` (saved in `review_same_price.json`).

**STOCK-FILE qty>=5 CROSS-CHECK, same session (owner asked to verify a claim, not just take it on faith) — final corrected numbers: 808 of 811 published (live) products (99.6%) are backed by a `STOCK_2026.xls` row with qty>=5.** Only 2 genuine exceptions, plus 1 borderline:
- **No matching stock-file row at all** (came from a different/earlier source, not this stock file): `51137295356` (BMW 7-Series F01/F02 LCI chrome trim) and `34 11 6 764 540` (BMW front brake pad set — this is also one of the 10 same-price review items above, so it's flagged twice for different reasons).
- **Matches a stock row but under qty 5**: `4H0 698 451D` (stock qty 1, the same brake-pad-set SKU imaged earlier this session).
- **First-pass check was wrong and got corrected same-session**: an initial cross-check found 135 "no stock match" live items, but that was a real matching bug — many BMW SKUs already had their leading `C`/`T` prefix stripped by the earlier prefix-cleanup project, while `STOCK_2026.xls` still has the original prefixed codes (e.g. live `63147851578` = stock `C 63 14 7 851 578`, qty 10) — the base-OEM matcher wasn't stripping that leading letter, only the trailing `/SUPPLIER` tag. Fixed by also matching against the leading-letter-stripped form. **Lesson for any future SKU/stock cross-referencing on this store: always normalize BOTH a leading single-letter prefix (`C`/`T`) AND a trailing `/SUPPLIER` suffix before comparing two SKU sources** — matching on only one half undercounts real matches significantly (135 false negatives out of 811 in this case, a 17% error).
- Reusable script: `C:\Users\user\AppData\Local\Temp\claude\xlsxtool\check_live_vs_stock_v2.js` (the corrected version — use this one, not `check_live_vs_stock_qty.js`, which has the leading-letter bug).

**SESSION WRAP 2026-09-15 (this session's actual final stopping point):**
- 445/458 known suffix-SKU fixes are live and confirmed; 13 remain, need owner review (see list above).
- 27 Audi/Porsche + 25 BMW live-image gaps filled this session (see the image-sourcing section of [[project_laser_auto_parts_catalog_quality]] for full detail); checkpoint for the ~35 still-imageless BMW items saved at `C:\Users\user\AppData\Local\Temp\claude\xlsxtool\bmw_live_no_img.json`.
- The two pending "update existing" import files in `C:\Users\user\Downloads\` (Febi ATF 1-row, BMW-14-groups 2279-row) are STILL unimported as of this wrap — not touched this session, still awaiting the owner's go-ahead.
- The 115-item Stock Expansion Phase 2 new-product research is still just research (84 of 115 clean after dedup) — no xlsx built, nothing created.
- The ready-to-paste full-catalog-cleanup prompt earlier in this file is still the correct next-session starting point — the SKU-suffix portion of it is now ~97% done, update that prompt's framing if resuming (it was written assuming 0% done).

Known open items to fold in if not already resolved by the time this runs:
- 166 `/SUPPLIER`-suffix SKUs already identified (71 published, 95 hidden)
  as of 2026-09-15 — re-verify count against the fresh export, don't trust
  this number as still current. **Shortcut confirmed 2026-09-15: 63 of the
  71 published ones already have a pre-computed clean OEM value sitting in
  the Audi seed file's "OEM normalized" column** (cross-checked by exact
  match against the seed's "SKU as listed" column) — reuse those values
  directly as `new_sku` rather than re-deriving them; only 8 (all Audi
  Q7/Porsche Cayenne/VW Touareg cross-platform items with a 7L0/7P0/4M0/9Y0
  prefix: `7L0 407 182G/FEBI`, `7L0 907 637C/BREMBO`, `7L0 907 637C/SK`,
  `7P0 907 637/SK`, `7P0 907 637C/SK`, `9Y0 907 253B/WINGS`, `7L0 129
  620/FEBI`, `4M0 411 317/LEM`) had no seed match and need a fresh lookup.
  The 95 hidden-item suffix SKUs were not cross-checked against the seeds
  yet — worth the same shortcut check before re-deriving those too.
- Two pending "update existing" import files sitting unimported in
  C:\Users\user\Downloads\ (`import_ready_83220144137_Febi_update.xlsx`,
  `import_ready_BMW_14groups_Porsche_Audi_update.xlsx`) — check with the
  owner whether to fold their edits into this pass or import them
  separately first; don't silently drop or duplicate that work.
- 115-item Stock Expansion Phase 2 new-product batch — research done,
  84 of 115 are genuinely new/clean after removing 27 already-live matches
  and resolving 6 internal duplicate-SKU groups (real distinct supplier
  stock lines, not created yet) — see this file's own notes above for the
  exact per-SKU supplier breakdown. This is CREATING new products, a
  different operation from the 4 fixes above which only touch existing
  live items — don't conflate the two lists.
- ~35 BMW live products still missing images (checkpoint file:
  C:\Users\user\AppData\Local\Temp\claude\xlsxtool\bmw_live_no_img.json) —
  regenerate via the public API sweep if this file is gone.

Cost discipline: this is a full-catalog pass across ~800+ published items
minimum — batch it (background forks of ~30-50 items for research-heavy
steps 2/3, matching the BMW gap-fill's proven pattern), use the bulk-import
Excel mechanism instead of per-item API calls wherever possible (this is
what ended three straight session-rate-limit hits on 2026-09-15), and check
in with the owner before any batch that isn't clearly bounded.
```

---

**Session wrap 2026-09-15 (ended early — owner had trouble connecting Remote Control and asked to save everything and end the session, not because the work itself was finished):** exact resume point —
- **Nothing new was imported this session.** The only *live* change is the ZF half of the 83220144137 ATF fluid (see above); everything else described in this update (Febi half, all 14 BMW groups, G555502M2, 83222365987) exists only as the two Excel files in Downloads, both already verified by reparsing before being written:
  1. `import_ready_83220144137_Febi_update.xlsx` (1 row)
  2. `import_ready_BMW_14groups_Porsche_Audi_update.xlsx` (34 rows)
- **Next action when resuming:** confirm with the owner whether to import both files now via Zid dashboard → Products → ⋮ → Import Products → update-existing-by-SKU, or whether they still want to fold in more changes first (their own words earlier this session: "will make some more changes then we can import" — that intent may still stand, don't assume it lapsed).
- **83212365930** remains flagged and untouched (separate SKU/data-integrity issue, not a compatibility-table case) — don't accidentally sweep it into a future "import everything" pass.
- If the two Excel files are no longer present in `C:\Users\user\Downloads\` when resuming (e.g. different machine, cleaned temp), they will need to be rebuilt — the full method (FAPI verification, per-group model/chassis/year merging, category mapping) is documented above and in [[feedback_zid_product_edit_method_rule]] in enough detail to redo without re-researching the underlying fitment facts (those are captured here).
