## Proof of Concept Scope (Technical Summary)

This sprint delivers our **Surface Product proof of concept** for an autonomous compliance follow-up agent. The purpose of this proof of concept is not to demonstrate a complete fraud-analysis system, but to validate that our **core technical approach—agent orchestration with real external actions—is feasible** and can be executed end-to-end in a live-deployed environment.

### What “working” means for the primary interaction

Our **Primary User Interaction** is:

> From a single instruction, the agent autonomously researches a compliance-related topic, generates a structured report, and executes follow-up actions such as emailing the report and scheduling a meeting.

For A4, we define “working” as meeting all of the following conditions during a live run:

1) **Single-instruction execution**
- The user provides one natural-language instruction that includes the topic to research and the intended follow-up actions (email and meeting scheduling).
- The system correctly interprets the instruction without requiring the user to manually break it into separate steps.

2) **Real research → real report generation**
- The agent performs web research (not a hardcoded response) and produces a structured report summary (e.g., key findings, bullet-point insights, sources/links if supported).
- The report is displayed back to the user in the product UI in a readable format that does not require manual rewriting to be usable as a starting point for communication.

3) **Real side effects (no simulation)**
- The agent triggers at least one real follow-up action (and in our baseline flow, both):
  - **Email**: sends the generated report (or a report summary) to a specified recipient.
  - **Calendar**: schedules a meeting based on the request (duration/time window or explicit time).
- The product provides an in-app confirmation/receipt (e.g., email recipient/subject; event title/time) to make the action auditable.

4) **End-to-end flow completes on live deployment**
- The entire workflow runs successfully on the deployed system without manual intervention, developer-only actions, or hidden steps.
- Failures, if they occur, must be surfaced to the user as explicit errors (not silent failure), with a clear indication of which action failed.

5) **Architectural boundaries are enforced**
- The system is structured with clear separation of concerns:
  - **Frontend**: chat interface for collecting the instruction and displaying results.
  - **Backend**: agent orchestration (planning + tool execution + result formatting).
  - **External services/tools**: web research access, email provider, and calendar provider.
- Configuration uses environment variables (no hardcoded secrets) and the system topology diagram shows how data and requests flow between these components.

### Quantitative definition of “good enough” for this sprint (required path)

Because this is a surface product baseline, we define a practical validation gate based on the required path only (excluding optional verification steps such as opening Gmail/Calendar to confirm results manually):

- **Task completion time (required path):** ≤ 900 seconds (15 minutes)  
  Baseline observed: 678 seconds (11m 18s)
- **Context switches (required path):** ≤ 1  
  Baseline observed: 0 (optional verification excluded)
- **Severe friction steps (required path):** 0  
  Baseline observed: 0 (per-step severity ratings)

These thresholds are intentionally permissive for A4 to reflect early-stage execution while still detecting catastrophic usability or reliability issues.

### Known technical constraints and accepted limitations (A4)

To keep the proof-of-concept scope realistic and falsifiable, we explicitly accept the following constraints in A4:

1) **No flagged-transaction ingestion yet**
- The system does not yet ingest or process real transaction feeds (e.g., webhook events, CSV uploads, or dashboard exports).
- Fraud pattern analysis is deferred; this sprint validates the follow-up orchestration layer that will later operate on flagged transactions.

2) **No file/document upload**
- The UI is chat-only and does not support uploading supporting evidence (receipts, screenshots, spreadsheets, or logs). Any “document output” is limited to generated text and links.

3) **Limited control and reversibility**
- Current baseline reveals a major constraint: once execution begins, users may not be able to stop/cancel or edit intermediate outputs.
- Follow-up actions (sending email, scheduling meetings) may be effectively irreversible without additional confirmation controls.

4) **Limited transparency during execution**
- During research/report generation, the system may appear as a black box (“…” / running) without step-by-step status visibility. This impacts trust and perceived responsiveness.

5) **Verification may require optional context switches**
- Users may choose to verify results externally (open Google Docs, email inbox, calendar). These are treated as optional in A4 and documented as baseline friction rather than required workflow steps.

6) **Dependence on third-party service reliability**
- Email/calendar execution depends on external provider availability and authentication state. Failures may occur due to expired auth, permission issues, or rate limits; these must be surfaced clearly to the user.

### Why this scope is valid for A4 and not a “toy”

This proof of concept is not a generic chatbot demo because it validates an agent system that:
- Converts one instruction into a multi-step plan,
- Produces a structured compliance report from real research,
- Executes real external actions (email + calendar),
- And enforces architectural boundaries that can scale toward the intended fraud-analysis product.

In subsequent demos, we will expand this foundation by adding transaction ingestion, persistence, and domain-specific fraud pattern analysis while reducing the friction discovered in this baseline (e.g., adding confirmation gates, better execution status, and fewer context switches).