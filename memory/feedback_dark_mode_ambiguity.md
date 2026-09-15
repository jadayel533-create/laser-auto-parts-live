---
name: feedback-dark-mode-ambiguity
description: "\"we want dark mode\" is ambiguous between a permanent redesign and a visitor toggle — clarify before building
metadata:
  type: feedback
---

When a user says "we want dark mode" (or similarly terse phrasing) for a website/app, do not assume which of two very different things they mean:
1. Permanently redesign the site/app to use dark colors for everyone (a settings/color change).
2. Add a switchable light/dark toggle that visitors control themselves (a real feature build — persistence, a button, both color sets, no flash-of-wrong-theme).

**Why:** On [[project_laser_auto_parts]] (2026-09-12), "we want dark mode" was taken as #1 — found the Zid نمو theme's color-scheme settings, built a dark palette, and published it live to the whole storefront. The user's follow-up ("wheres the darkmode button") revealed they meant #2 the whole time. Had to immediately revert the live site back to its original colors.

**How to apply:** Before implementing "dark mode" (or any similarly overloaded UI request — "make it responsive," "add search," "make it faster" can hide the same ambiguity), ask which variant is meant, or at minimum implement in a reversible/draft state and confirm before publishing anything live. Don't let "I found a way to technically satisfy the literal words" substitute for confirming actual intent, especially for a change that's visible to every visitor the moment it ships.
