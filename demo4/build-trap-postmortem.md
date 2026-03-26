# Build Trap Post-Mortem — PyGuard PDF Report Sprint

**Sprint end:** March 24, 2026  
**Feature reviewed:** LaTeX PDF report generation with embedded matplotlib visualizations  
**Pivot contract hypothesis:** PDF reports with 5 charts and 6 structured sections are significantly more actionable than raw Slack text delivery.

---

## Did the Feature Deliver the Hypothesized Value?

Partially. The technical implementation succeeded: a 547KB LaTeX-compiled PDF with 5 embedded charts is generated and uploaded to Slack within a single pipeline run. The kill metric threshold (80% of runs producing verified, non-rejected PDFs) was met in the automated test suite — PDF generation passed 100% of isolated unit tests and the end-to-end test produced a correct output in 5.3 seconds.

However, the full hypothesis cannot be confirmed as validated. The pivot contract defined value in terms of user perception — executives finding the report "more actionable" — and no real SME executive has yet interacted with the system. We built the delivery mechanism but did not validate the demand side. We know the PDF is technically correct. We do not yet know whether it materially changes how an operations lead acts on fraud findings.

---

## Was Building Necessary to Learn What We Learned?

No — not fully. The core capability questions (can we generate charts from real data? can tectonic compile LaTeX? can Slack accept file uploads?) were genuinely unknown and required building to answer. Those were valid build decisions: they validated capability constraints that could not be resolved by user interviews or mockups.

The format-preference question — PDF versus structured Slack message versus Block Kit with attached images — did not require a full build to test. A Figma mockup of the PDF shown to 3–5 SME operations leads during a 20-minute interview would have validated whether the format itself resonated before investing in LaTeX templating and tectonic infrastructure. We skipped that step and assumed the answer.

This is a partial build trap on the demand side: the LaTeX compilation and chart generation pipeline was necessary work, but committing to the full PDF delivery format before validating the format preference with a single user was premature.

---

## What Assumptions Drove Premature Building?

Two assumptions were made without validation:

**Assumption 1:** A structured PDF is the right delivery artifact for SME executives. This felt obvious — PDFs are professional, portable, and printable — but "obvious" is not validated. An SME operations lead who lives in Slack may find a well-formatted Block Kit message with pinned charts faster to act on than a PDF they need to download and open. This should have been a research question before it became a build decision.

**Assumption 2:** Visualizations are essential for actionability at alpha stage. This may be true, but it drove significant complexity (matplotlib pipeline, LaTeX embedding, tectonic installation) that could have been deferred. A text-only executive summary with strong numbers is cheaper to produce and may deliver sufficient value for early alpha feedback.

---

## What Will Change in the Next Pivot Contract?

The next pivot contract will separate demand validation from capability validation explicitly. Before committing to any new delivery format, a structured user test with at least 3 SME personas will be required. The kill metric will include a user-facing signal — for example, "at least 2 of 3 test users take a documented action based on the report within 24 hours" — not just a technical correctness check. Capability hypotheses (can we build it?) will remain in the contract, but they will be gated behind demand hypotheses (do users want it?) to prevent building ahead of validation.

---

## Features Deprioritized and Why

The DataAnalystAgent (code interpreter for on-demand CSV analysis) and WhatsApp integration were both deprioritized. The DataAnalystAgent was excluded because the fraudDataAnalystAgent already covers the primary analytical queries through deterministic SQL tools, making a code-execution layer redundant for alpha. WhatsApp was deprioritized because Slack is the validated primary channel and completing the bridge Python client adds infrastructure risk with no corresponding user demand signal in the alpha scope. Both decisions were correct: they deferred complexity that would not have improved the primary CUJ outcome.
