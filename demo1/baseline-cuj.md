# Baseline CUJ & Audit

## User Goal and Persona

### Persona

- **Name:** Bobby Blank
- **Role:** Fraud & Compliance Operations Analyst at a mid-sized e-commerce company (Shopify-based)
- **Context:** Triages potential fraud/return-abuse signals and prepares short compliance briefs for internal stakeholders (ops lead, finance, legal/compliance).
- **Constraints:** High workload; frequent context switching between browser research, email drafting, and calendar scheduling; needs fast, consistent follow-ups without spending time writing and coordinating.
- **Motivation:** Reduce manual “glue work” (research + summarizing + emailing + setting meetings) so she can focus on higher-value investigation and decisions.

### Value-Driven Goal

From one instruction, generate a compliance research report and complete follow-up actions (email the report and schedule a meeting) in **≤ 15 minutes**, with **≤ 1 context switch**, and without manual rewriting of the report.

## The Journey Audit (Baseline)


| Step # | Action (what the user does)                                                               | Context Switches (where they leave/shift)  | Time (seconds) | Friction Severity (None/Moderate/Severe) | Evidence (screenshot)                                   |
| -----: | ----------------------------------------------------------------------------------------- | ------------------------------------------ | -------------: | ---------------------------------------- | ------------------------------------------------------- |
|      1 | Open the deployed Surface Product URL                                                     | None                                       |              3 | None                                     | [initial-stage.png](assets/initial-stage.png)           |
|      2 | Type a single instruction (research → report → email + meeting) and submit              | None                                       |             60 | Moderate                                 | [prompt-typed.png](assets/prompt-typed.png)             |
|      3 | Wait while the agent executes (research + report generation)                              | Mental context shift (waiting/uncertainty) |            300 | Moderate                                 | [prompt-submitted.png](assets/prompt-submitted.png)     |
|      4 | Review the summary of the report                                                          | None                                       |            300 | Moderate (readability/time to scan)      | [report-summary.png](assets/report-summary.png)         |
|      5 | (Optional verification) Open Google Doc link and confirm the document content             | Leaves product → Google Docs              |            300 | Moderate                                 | [document-confirmed.png](assets/document-confirmed.png) |
|      6 | Confirm the email action by checking the summary about the email                          | Might open inbox to verify                 |              5 | Moderate                                 | [email-summary.png](assets/email-summary.png)           |
|      7 | (Optional verification) Open email inbox and confirm                                      | Leaves product → Email                    |             30 | Moderate                                 | [email-confirmed.png](assets/email-confirmed.png)       |
|      8 | Confirm the meeting scheduling action by checking the summary about the meeting scheduled | Might open calendar to verify              |              5 | Moderate                                 | [meeting-summary.png](assets/meeting-summary.png)       |
|      9 | (Optional verification) Open calendar and confirm event exists                            | Leaves product → Calendar                 |             30 | Moderate                                 | [meeting-confirmed.png](assets/meeting-confirmed.png)   |
|     10 | Return to product and conclude task (report delivered + meeting booked)                   | None                                       |              5 | None                                     | [prompt-submitted.png](assets/prompt-submitted.png)     |

## Analysis & Recommendations (Baseline)

### Highlights (None / Great)

- The single-instruction workflow completes end-to-end (research → report → email/meeting), matching the stated primary interaction.
- The generated report is structured and usable as a starting point without manual drafting from scratch.
- The system provides confirmations for follow-up actions (email sent / meeting created), allowing the user to proceed.

### Lowlights (Moderate)

**Prompt-format uncertainty (Step 2)**

- **Symptom:** Unclear how specific the instruction must be (recipient, time/date, duration, timezone).
- **Impact:** Increases completion time; risk of re-trying prompts.
- **Fix:** Provide 2–3 prompt templates + inline “required fields” hints.

**Low transparency during execution (Step 3)**

- **Symptom:** Black-box waiting; unclear which sub-step is running.
- **Impact:** Reduced trust; user may interrupt/re-submit.
- **Fix:** Step-based progress indicator (Researching → Drafting → Emailing → Scheduling) with status updates.

**External verification context switches (Steps 6 and 8)**

- **Symptom:** User leaves product to verify email/calendar.
- **Impact:** Breaks focus; adds time; increases context switches.
- **Fix:** Show stronger action receipts inside the product + direct links.

### Severe

- No stop/cancel/edit once the agent starts (a typo or wrong instruction can’t be corrected mid-run).
- Actions may be irreversible without confirmation (email/meeting can be sent/created without user approval).

## Product Recommendations (Next Iteration)

- Add Stop/Cancel + Edit capability during execution (prevents irreversible mistakes).
- Add “Review & Confirm” before sending email/scheduling (approval gate for side effects).
- Add prompt templates + guided input (reduce prompt uncertainty).
- Add step-based execution status + action receipts (reduce black-box waiting and verification switches).

## Quantitative Success Metrics (Required Path Only)

### Completion Time (Required Path Only)

Include Steps: **1, 2, 3, 4, 6, 8, 10**

- Total time = Step 1 (3) + Step 2 (60) + Step 3 (300) + Step 4 (300) + Step 6 (5) + Step 8 (5) + Step 10 (5)
- **Total = 678 seconds = 11 minutes 18 seconds**

**A4 success threshold (gate):** ≤ 900 seconds (15 minutes)
**Gate result:** PASS (678s ≤ 900s)

### Context Switches (Required Path Only)

Optional verification steps (5, 7, 9) are excluded.

- Step 6: “might open inbox” but the user does not leave the product on the required path
- Step 8: “might open calendar” but the user does not leave the product on the required path

**Required-path context switches = 0**
**A4 threshold:** ≤ 1
**Gate result:** PASS

### Severe Friction Steps (Required Path Only)

**Severe friction steps = 0** (based on the per-step severity ratings in the table).
