# Feature Prioritization & Product Definition CUJ

## User Goal and Persona

### Persona

- **Name:** Bobby Blank
- **Role:** Fraud & Compliance Operations Analyst at a mid-sized e-commerce company (Shopify-based)
- **Context:** Triages potential fraud/return-abuse signals and prepares short compliance briefs for internal stakeholders (ops lead, finance, legal/compliance).
- **Constraints:** High workload; frequent context switching between browser research, email drafting, and calendar scheduling; needs fast, consistent follow-ups without spending time writing and coordinating. Often away from her desk or working from her phone.
- **Motivation:** Reduce manual "glue work" (research + summarizing + emailing + setting meetings) so she can focus on higher-value investigation and decisions — from wherever she is, without needing to open a separate app or remember a URL.

### Value-Driven Goal (Updated for Sprint 2)

From one instruction sent via WhatsApp, generate a compliance research report and complete follow-up actions (email the report and schedule a meeting) in **≤ 15 minutes**, with **≤ 1 context switch**, and without manual rewriting of the report — all from the messaging app Bobby already uses every day.

---

## Product Definition CUJ (Sprint 2 — WhatsApp as Primary Interface)

### What Changed from Baseline

In Sprint 1, Bobby interacted with PyGuard through a deployed web URL. This sprint replaces the web chat frontend with WhatsApp as the primary surface product. The core agent pipeline (Memory Agent → Orchestrator → sub-agents) is unchanged; what changed is how the user reaches it and receives results.

Key shifts:

- **Entry point:** Web URL → WhatsApp (an app Bobby already has open)
- **Onboarding:** None previously → one-time QR code link to pair the bridge
- **Response format:** Styled web chat bubbles with markdown → plain WhatsApp text messages
- **Availability:** Desktop browser only → phone, desktop WhatsApp, WhatsApp Web (any device)

### Journey Map

| Step # | Action (what the user does) | Context Switches | Time (seconds) | Friction Severity | Notes |
| -----: | --- | --- | ---: | --- | --- |
| 0 | (One-time) Scan QR code to link WhatsApp to PyGuard bridge | Opens camera / WhatsApp Linked Devices | 30 | Moderate | One-time onboarding; not repeated |
| 1 | Open WhatsApp (already on phone or desktop) | None — already in WhatsApp | 2 | None | Replaces "open deployed URL" (was 3s, now 2s) |
| 2 | Type a single instruction and send it to PyGuard's number | None | 45 | None | Natural texting; no UI to learn. Faster than web (was 60s) |
| 3 | Wait while the agent executes (research + report generation) | Mental context shift (waiting/uncertainty) | 300 | Moderate | Same execution time; no progress indicator in WhatsApp |
| 4 | Receive and read the text summary of the report in WhatsApp | None | 120 | Moderate | Plain text only; no rich formatting. Faster scan than web (was 300s) because WhatsApp messages are more concise |
| 5 | (Optional) Tap Google Doc link in the message to verify | Leaves WhatsApp → Google Docs | 300 | Moderate | Same as baseline |
| 6 | Read the email confirmation summary in the same WhatsApp chat | None | 5 | None | No context switch; inline in conversation |
| 7 | (Optional) Open email inbox to verify | Leaves WhatsApp → Email | 30 | Moderate | Same as baseline |
| 8 | Read the meeting confirmation summary in the same WhatsApp chat | None | 5 | None | No context switch; inline in conversation |
| 9 | (Optional) Open calendar to verify event | Leaves WhatsApp → Calendar | 30 | Moderate | Same as baseline |
| 10 | Continue conversation or close WhatsApp | None | 2 | None | No "return to product" needed; user is already in their messaging app |

### Quantitative Success Metrics (Required Path: Steps 1–4, 6, 8, 10)

**Completion Time:**

- Total = Step 1 (2) + Step 2 (45) + Step 3 (300) + Step 4 (120) + Step 6 (5) + Step 8 (5) + Step 10 (2) = **479 seconds = 7 minutes 59 seconds**
- Sprint 1 baseline: 678 seconds (11 min 18s)
- **Improvement: 199 seconds faster (29% reduction)**
- A4 threshold (≤ 900s): **PASS**

**Context Switches (Required Path):**

- Required-path context switches = **0** (all confirmations arrive in the same WhatsApp chat)
- A4 threshold (≤ 1): **PASS**

**Severe Friction Steps (Required Path):**

- Severe friction steps = **0**

---

## Analysis: What the WhatsApp Channel Improved

### Friction Eliminated

| Baseline Friction Point | How WhatsApp Resolves It |
| --- | --- |
| Must remember and open a separate URL (Step 1) | WhatsApp is already open on Bobby's phone/desktop; PyGuard is a contact, not a URL |
| Web-only; no mobile access | WhatsApp works on phone, desktop, and web — Bobby can issue tasks from anywhere |
| Context switch to return to product after verifying (Step 10) | The conversation lives in WhatsApp; there is no "product" to return to |
| Unfamiliar chat UI | Bobby already knows how to send a WhatsApp message |

### Friction Reduced

| Baseline Friction Point | Improvement |
| --- | --- |
| Prompt-format uncertainty (Step 2) | Texting in WhatsApp feels conversational; Bobby naturally writes shorter, clearer messages (45s vs. 60s) |
| Response readability (Step 4) | WhatsApp forces concise text; less visual noise than a full web page (120s vs. 300s to scan) |

### Friction Unchanged or Introduced

| Friction Point | Status | Detail |
| --- | --- | --- |
| Black-box waiting during execution (Step 3) | Unchanged | WhatsApp has no built-in progress bar; the user still waits ~5 min with no feedback |
| No stop/cancel/edit mid-execution | Unchanged | Once the message is sent, the agent runs to completion |
| No confirmation before irreversible actions | Unchanged | Email/meeting may execute without approval gate |
| One-time QR onboarding (Step 0) | New | 30 seconds of moderate friction, but only once per device |
| Plain text only — no rich formatting | New | WhatsApp does not render markdown tables, headings, or styled content; reports are plain text |
| Voice message not supported | New | Bobby cannot send voice notes to PyGuard; only text is processed |

---

## Feature-to-Value Mapping

| # | Feature | Journey Step | Problem / Friction | Hypothesis | Success Metric | Priority | Sprint |
| ---: | --- | --- | --- | --- | --- | --- | --- |
| F1 | WhatsApp channel integration | Steps 1, 2, 10 | Users must open a separate web URL; no mobile access; unfamiliar UI | Replacing the web frontend with WhatsApp will reduce entry friction and enable mobile access | Entry time ≤ 5s; task completion from phone | P0 | Sprint 2 (built) |
| F2 | Multi-agent pipeline over WhatsApp | Steps 2–4, 6, 8 | Core agent capabilities (research, email, calendar, docs, data) must work identically over WhatsApp | Routing WhatsApp messages through the existing Memory → Orchestrator pipeline produces the same quality results as the web UI | Same task completion rate as web; all 5 sub-agents functional over WhatsApp | P0 | Sprint 2 (built) |
| F3 | Persistent conversation history per WhatsApp user | Steps 2, 3, 4 | Without history, the agent cannot maintain context across messages; user must repeat information | Storing WhatsApp messages with `source="whatsapp"` in the existing DB enables multi-turn context | Agent correctly references prior messages in follow-up requests | P0 | Sprint 2 (built) |
| F4 | Phone-number-based user identity | Steps 2, 6, 8 | Agent needs to know who "me" is to send emails on the user's behalf and schedule meetings for them | Mapping the WhatsApp sender phone number to a DB user record lets the agent resolve "send email to me" correctly | Agent uses correct sender email/phone without user re-stating identity | P1 | Sprint 2 (built) |
| F5 | Cross-channel history awareness | Steps 2, 3 | User may have previously used the web chat; agent should not lose that context when they switch to WhatsApp | Fetching the last 5 messages from different sources (existing DB query) gives the agent cross-channel awareness | Agent references web-chat context when responding on WhatsApp | P1 | Sprint 2 (built) |
| F6 | Allowed-sender list (access control) | Step 2 | Without access control, anyone who knows the PyGuard number can use it | An `allow_from` configuration restricts which phone numbers can interact with the agent | Unauthorized senders receive no response; authorized senders work normally | P1 | Sprint 2 (built) |
| F7 | Auto-reconnect on bridge disconnect | Step 3 | If the WhatsApp bridge drops, in-flight and future messages are lost | 5-second automatic reconnect loop ensures the bridge recovers without manual intervention | Bridge reconnects within 10s of disconnection; no message loss on recovery | P1 | Sprint 2 (built) |
| F8 | Progress/status messages during execution | Step 3 | Black-box waiting for ~5 minutes with no feedback reduces trust and may cause re-submission | Sending intermediate WhatsApp messages ("Researching...", "Drafting report...", "Sending email...") reduces perceived wait time | User reports higher confidence; re-submission rate drops | P2 | Sprint 3 (deferred) |
| F9 | "Working on it" acknowledgement | Step 3 | User sends a message and sees nothing for minutes; may think it was not received | Immediately replying "Got it — working on this now" confirms receipt | Time-to-first-response ≤ 2s | P2 | Sprint 3 (deferred) |
| F10 | Confirmation before irreversible actions | Steps 6, 8 | Emails and meetings execute without user approval; a typo in the instruction leads to an unwanted real action | Adding a "Shall I send this?" approval step before email/calendar actions prevents mistakes | Zero unintended emails/meetings sent; user approves each action | P2 | Sprint 3 (deferred) |
| F11 | Rich media in WhatsApp responses | Step 4 | Reports are plain text; tables, headings, and structured data lose formatting | Sending Google Doc links prominently and formatting key data with WhatsApp-supported bold/italic improves readability | User reads report summary in ≤ 90s (down from 120s) | P2 | Sprint 3 (deferred) |
| F12 | Voice message transcription | Step 2 | Bobby cannot dictate tasks by voice; must type everything | Transcribing WhatsApp voice notes to text before passing to the agent enables hands-free task input | Voice-initiated tasks complete with same success rate as text tasks | P3 | Future (deferred) |
| F13 | File and image attachment handling | Step 2 | Bobby cannot share a screenshot or CSV for the agent to analyze | Processing WhatsApp media attachments and passing them to the data analyst sub-agent enables file-based workflows | Agent correctly processes attached files in ≥ 80% of attempts | P3 | Future (deferred) |
| F14 | WhatsApp group chat support | Steps 1, 2 | Bobby's team cannot collaborate with PyGuard in a shared group; only 1:1 chats work | Supporting group messages (with @mention trigger) enables team-wide agent access | Agent responds correctly in group chats when mentioned | P3 | Future (deferred) |

---

## Strategic Prioritization Rationale

### P0 — Must Have (Sprint 2, Built)

Features F1–F3 are the minimum required for the WhatsApp channel to function at all. Without them, no user interaction is possible over WhatsApp.

- **F1** replaces the entire web frontend and is the architectural foundation of this sprint.
- **F2** ensures the agent's capabilities are not degraded by the channel change. If the pipeline does not work over WhatsApp, the product has regressed.
- **F3** is required for any multi-turn interaction. Without history, every message is treated as the first message, making the agent unable to handle follow-ups or context-dependent tasks.

### P1 — Should Have (Sprint 2, Built)

Features F4–F7 are required for the product to be usable beyond a simple demo.

- **F4** is critical because the agent's most common tasks (send email, schedule meeting) require knowing the sender's identity. Without it, the agent cannot resolve "send email to me."
- **F5** ensures users who previously used the web chat do not lose their history when switching to WhatsApp.
- **F6** is a basic security measure. Without it, the WhatsApp number is an open API endpoint accessible to anyone.
- **F7** prevents service outages from requiring manual intervention.

### P2 — Nice to Have (Sprint 3, Deferred)

Features F8–F11 address the friction points identified in the CUJ analysis that remain unresolved from the baseline. They improve trust, safety, and readability but are not blockers for core functionality.

- **F8 and F9** address the black-box waiting problem (Step 3), which was identified as moderate friction in both Sprint 1 and Sprint 2. Deferred because the core flow works without them.
- **F10** addresses the severe friction point from Sprint 1 (irreversible actions without confirmation). Deferred because it requires significant orchestrator refactoring to add an approval gate mid-execution.
- **F11** addresses plain-text readability limitations. Deferred because it is a UX polish item, not a functional blocker.

### P3 — Future (Deferred Indefinitely)

Features F12–F14 expand the product's surface area beyond text-based 1:1 chat. They are valuable but represent new interaction paradigms that require significant engineering investment and are not tied to the core CUJ.

---

## Implementation Timeline

### Sprint 2 (Current — Completed)

| Feature | Status | Implementation Detail |
| --- | --- | --- |
| F1: WhatsApp channel integration | Done | Node.js bridge (Baileys) + Python WebSocket service; replaces web frontend |
| F2: Multi-agent pipeline over WhatsApp | Done | WhatsApp service calls `process_chat_message()` — same Memory → Orchestrator pipeline |
| F3: Persistent conversation history | Done | Messages stored with `source="whatsapp"` in SQLite; history queries work per-source |
| F4: Phone-number-based user identity | Done | WhatsApp sender ID mapped to `user` field; `get_user_by_phone()` resolves identity |
| F5: Cross-channel history awareness | Done | Existing DB query fetches last 5 messages from different sources automatically |
| F6: Allowed-sender list | Done | `WHATSAPP_ALLOW_FROM` env var; empty = allow all, populated = allowlist |
| F7: Auto-reconnect | Done | 5-second retry loop in `WhatsAppService.start()` on connection loss |

### Sprint 3 (Next — Planned)

| Feature | Estimated Effort | Dependencies |
| --- | --- | --- |
| F8: Progress/status messages | Medium | Requires hooks in orchestrator sub-agent execution to emit intermediate messages |
| F9: "Working on it" acknowledgement | Small | Send immediate reply in `WhatsAppService._handle_inbound()` before calling pipeline |
| F10: Confirmation before irreversible actions | Large | Requires orchestrator refactoring: pause execution, send confirmation, wait for user reply, resume |
| F11: Rich media formatting | Small | Use WhatsApp-supported formatting (bold, italic, lists) in response text |

### Future Sprints

| Feature | Estimated Effort | Dependencies |
| --- | --- | --- |
| F12: Voice message transcription | Medium | Integrate speech-to-text API; bridge must forward audio binary |
| F13: File/image attachments | Large | Bridge must download media; agent must route to data analyst sub-agent |
| F14: Group chat support | Medium | Bridge must handle group message parsing; agent needs @mention trigger logic |

---

## Evolved System Topology (Sprint 2)

The system architecture matured from a simple HTTP request/response model to a dual-interface architecture with a persistent bridge layer:

**Sprint 1 (Baseline):**

```
Browser → FastAPI (POST /api/chat) → Memory Agent → Orchestrator → Sub-agents → Response → Browser
```

**Sprint 2 (Current):**

```
WhatsApp User
    ↕ (WhatsApp Web Protocol)
Node.js Bridge (Baileys + WebSocket on localhost:3001)
    ↕ (WebSocket)
Python WhatsApp Service
    ↕ (calls process_chat_message)
FastAPI Backend
    ↕
Memory Agent → Orchestrator → Sub-agents (Calendar, Email, Research, Report, Data)
    ↕
Response → WhatsApp Service → Bridge → WhatsApp User
```

Key architectural additions:

- **Node.js bridge layer**: Handles WhatsApp Web protocol via Baileys; communicates with Python over WebSocket. Runs as a separate process bound to localhost.
- **Python WhatsApp service**: Long-running async task started in FastAPI's lifespan. Connects to bridge, receives messages, feeds them into the existing agent pipeline, and sends responses back.
- **Shared chat handler**: Extracted from the HTTP route into `chat_handler.py` so both the API endpoint and the WhatsApp service call identical processing logic.
- **Source-aware database**: Messages tagged with `source="whatsapp"` enable per-channel history and cross-channel awareness without schema changes.
