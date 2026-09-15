---
name: feedback-verify-numeric-claims-live
description: "Never state a specific real-world number (price, fee, delivery time) in customer-facing content sourced from a settings/config page without cross-checking against actual live behavior first"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: bd9e7098-a236-4415-97ef-572768b4d620
  modified: 2026-09-14T01:15:38.813Z
---

Caught 2026-09-14 on Laser Auto Parts: pulled a shipping fee ("24 SAR + 10 SAR COD") from the store's public `/shipping-and-payment` settings-display page and put it directly into new customer-facing product-page text. The owner checked their own real checkout and got 22.50 SAR — the settings page was stale/wrong, unrelated to the actual charged amount. Had to walk the number back twice: first removed it (safe but vague), then found the REAL number in a different, more authoritative source (the shipping-methods marketplace dashboard page, which matched the real checkout exactly) and restored a specific figure only once independently confirmed by two matching sources.

**Why:** a settings/config/admin display page can silently drift out of sync with what a live system actually does (dynamic pricing, cached pages, stale merchant-entered reference values) — it looks authoritative but isn't guaranteed to be. Publishing a wrong specific number is worse than publishing no number at all, especially for anything money-related.

**How to apply going forward, on any project:** before writing a specific price, fee, delivery estimate, or similar real-world number into any customer-facing or otherwise "published as fact" content, either (a) verify it against the actual live/runtime behavior (a real checkout, a real API response, a real calculation) rather than trusting a dashboard/settings display alone, or (b) if that's not practical, use non-numeric accurate language instead ("calculated at checkout," "varies by destination") rather than guessing or trusting an unverified source. When two independent sources agree (e.g., a config page AND a live checkout both show the same number), that's when it's safe to state specifically — a single source, especially a settings/admin display, is not enough on its own for anything with real financial stakes.
