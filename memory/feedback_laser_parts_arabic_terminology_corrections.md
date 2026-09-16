---
name: feedback-laser-parts-arabic-terminology-corrections
description: "Laser Auto Parts Arabic naming rules - two specific words the owner flagged as wrong/inappropriate, with their required replacements, for use in all future product name/description text"
metadata:
  node_type: memory
  type: feedback
  originSessionId: 56dfeb1c-b3f8-4b86-836e-9007960cd6b4
  modified: 2026-09-15T18:41:27.492Z
---

Owner corrections (2026-09-15) to the established Arabic common-name dictionary (see [[project_laser_auto_parts_catalog_quality]]) — apply these going forward for every future product create/update, not just the one-time catalog fix that triggered them:

1. **`قضيب` (a literal "rod") must never be used — the owner said it reads as an inappropriate/vulgar word in this context, even though "قضيب التوجيه" is technically the standard Arabic term for "tie rod."** Found in the catalog in two distinct contexts, each needing a different replacement (never a blind find-replace of the bare word):
   - **Tie rod context** (`قضيب توجيه` / `قضيب التوجيه`, including inside `طقم قضيب التوجيه` for a tie-rod set): replace the whole phrase with **`ذراع التوجيه`** (owner's explicit instruction: "ذراع tie rod"). Example: `طقم قضيب التوجيه` → `طقم ذراع التوجيه`.
   - **Anti-roll bar / stabilizer bar context** (`قضيب الموازنة`, `قضيب موازنة`, `قضيب موازن`, `رابط قضيب`, `جلبة ثبات القضيب`): drop `قضيب`/`القضيب` entirely and keep just the `موازن`/`الموازنة` word already present, or insert `موازن` in its place when no such word is already in the phrase. Example: `دعامة قضيب الموازنة الأمامي` → `دعامة الموازنة الأمامي`; `رابط قضيب خلفي` → `رابط موازن خلفي`; `جلبة ثبات القضيب الأمامي` → `جلبة ثبات الموازن الأمامي`. This is consistent with the store's own pre-existing dictionary terms for this part family (`جوزة موازنة` for stabilizer link, `بوش الموازن` for stabilizer bush) — those never used `قضيب` in the first place.

2. **`وهب` is a mistranslation of "hub" (as in wheel hub bearing) — it's a real but unrelated Arabic word ("granted/gifted"), phonetically similar but wrong.** Replace with **`هوب`** (the correct phonetic transliteration), e.g. `طقم محمل وهب العجلة الأمامي` → `طقم محمل هوب العجلة الأمامي` (Front Wheel Hub Bearing Kit).

**How to apply:** when writing or auditing any Arabic name/description/short-description/keywords text for a new or existing product, never introduce `قضيب` or `وهب` — use the replacements above instead, chosen by context per the rules here rather than a single global substitution.
