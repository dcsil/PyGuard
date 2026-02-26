# Pivot Contract — Sprint 2 (WhatsApp Channel)

**HYPOTHESIS:** Replacing the web chat frontend with WhatsApp as the primary interface will reduce end-to-end task completion time by at least 25% (from the baseline of 678 seconds to ≤ 508 seconds) while maintaining zero required-path context switches, because users can issue instructions from an app they already have open without learning a new UI or remembering a URL.

**KILL METRIC:** If fewer than 80% of test runs (n ≥ 10) complete the full required-path CUJ (send instruction → receive report summary → receive email confirmation → receive meeting confirmation) in under 510 seconds with zero required-path context switches, we pivot.

**TRIGGER DATE:** End of Demo 2 (Febuary, 2026).

**FALLBACK OPTIONS:**
1. Revert to the web chat frontend and instead invest in guided prompt templates and a step-based progress indicator to reduce the baseline friction points (same problem — reducing task completion time — different solution surface).
2. Keep WhatsApp as a secondary notification channel only (receive results/confirmations) while retaining the web chat as the primary input surface (different interaction model within the same channel investment).
