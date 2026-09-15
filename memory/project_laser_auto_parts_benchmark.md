---
name: project-laser-auto-parts-benchmark
description: "Laser Auto Parts — published competitive benchmark artifact vs Saudi + Gulf-wide auto-parts competitors, with prioritized recommendations. Live 2026-09-14."
metadata: 
  node_type: memory
  type: project
  originSessionId: bd9e7098-a236-4415-97ef-572768b4d620
  modified: 2026-09-14T01:14:49.340Z
---

Part of [[project_laser_auto_parts]]. Published artifact: **"Laser Auto Parts Benchmark"** — https://claude.ai/code/artifact/1251977c-f19e-49e8-8fd6-aa3f36949e82 (private, owned by the user; update in place via the Artifact tool with this URL rather than creating a new one).

**Scope:** two research passes merged into one report.
1. **Saudi pass** — us vs. Speero, Mkena, autoparts.sa, Saudi Parts Store, plus Noon Automotive as a lower-confidence marketplace reference. 7-dimension rubric (catalog, search/fitment, pricing transparency, trust signals, Arabic quality, design/mobile, SEO), 1-5 each.
2. **Gulf-wide pass** — same rubric extended to UAE/Kuwait/Qatar/Bahrain/Oman: PartSouq, Getgayar (Kuwait), PartsOnClick.ae (UAE), Partora (pan-GCC), Royal Spare Parts (Qatar). Bahrain/Oman: no dedicated online specialist found in either market (real gap, not a scoring miss).

**Final standing (corrected, see below):** tied 2nd in Saudi (25/35, with Speero) — ahead of autoparts.sa (our closest German-car rival) and Saudi Parts Store; **#1 among Gulf sites actually verified live** (25/35, ahead of Partora 20 and Royal Spare Parts 14 — PartSouq's higher 26 is an unverified estimate, its site blocked direct fetch).

**Real strengths confirmed:** prices shown directly with instant add-to-cart (unlike autoparts.sa, Partora, Royal Spare Parts which hide pricing behind WhatsApp/quote flows); 3 search entry points (brand→model, part number, VIN) — more than any competitor checked, genuinely rare regionally too; Arabic-first dictionary-consistent content stood out even more in the Gulf-wide pass (several regional competitors are English-only).

**CORRECTION MADE MID-SESSION, important process lesson:** the first draft wrongly scored our own Trust dimension low, citing "policy pages disabled/unpublished" — this was flat wrong. A fork doing the research checked Zid's `legal_pages` SETTINGS field (genuinely empty, per [[project_laser_auto_parts_legal_pages_footer]]) without knowing the real policy content lives in a separate Pages app and was already fixed/linked in the footer back in an earlier session. Live re-verification found all 4 policy pages resolve correctly. **Trust score corrected 2→3, total corrected 24→25 (moved us from tied-3rd to tied-2nd).** Lesson reinforced in [[project_laser_auto_parts_legal_pages_footer]]: any future audit of this store's "trust signals"/legal content must check the live footer + Pages app directly, never trust the settings-field check alone.

**Real remaining gap identified (not yet acted on):** product reviews/star ratings are genuinely absent — confirmed via live DOM check (no review UI on any product page), the one real trust-signal gap left after the correction above.

**Recommendations from the report already actioned this session:** OEM Verified badge (see [[project_laser_auto_parts_oem_badge]]), delivery/pickup info on product pages (see [[project_laser_auto_parts_delivery_pickup_info]]). **Not yet actioned:** enable product reviews, enable Tabby/Tamara BNPL, add a Year step to the homepage Make→Model widget, confirm CR number visibility, lead marketing copy with "search by VIN/part number" (a genuine regional advantage to promote, not a gap to fix).
