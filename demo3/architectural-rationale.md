# Architectural Rationale — Sprint 3 (Slack Integration)

## What Changed

The Sprint 2 messaging channel — a Node.js bridge process communicating over WebSocket with a Python `WhatsAppService` — was replaced entirely with a single Python `SlackService` using Slack's Socket Mode. Two new modules were added (`slack/config.py`, `slack/service.py`), one dependency layer was removed (`PyGuard/bridge/` Node.js process, `websockets` Python package), and two new dependencies were added (`slack-sdk`, `slackify-markdown`). The core agent pipeline (`chat_handler.process_chat_message()`) was untouched.

---

## Why It Was Necessary

The WhatsApp architecture had a structural problem that was not a bug: it required two processes to be running simultaneously. The Node.js bridge handled the WhatsApp Web protocol; the Python service connected to it over a local WebSocket. If the bridge crashed, the Python service would log a warning and retry, but any messages sent during the gap were silently lost. There was no message queue, no durability guarantee, and no way to recover in-flight requests. Beyond reliability, WhatsApp itself turned out to be the wrong channel — a consumer app for a workflow that lives entirely inside a corporate Slack environment. The architecture was sound for the problem it solved; it was just solving the wrong problem.

---

## Alternatives Considered

- **Slack Webhook mode instead of Socket Mode.** Webhook mode requires a publicly reachable HTTPS URL. That means either deploying to a server or running a tunnel (ngrok) locally. Socket Mode requires no public endpoint — the client opens an outbound WebSocket to Slack's infrastructure. For a development-stage product running on localhost, Socket Mode was the only viable option that does not require standing up additional infrastructure.

- **Keeping the bridge pattern and swapping WhatsApp for Slack inside it.** We could have kept the Node.js intermediary and replaced Baileys (WhatsApp library) with a Node.js Slack SDK, maintaining the same WebSocket-to-Python handoff. This would have preserved architectural symmetry with Sprint 2 but reproduced the two-process reliability problem with no benefit. Slack has a mature Python SDK; there was no reason to introduce a Node.js layer.

- **Using an existing Slack bot framework (Bolt for Python).** Slack's Bolt framework wraps the SDK with route-style event handling. We chose the raw `slack-sdk` instead because Bolt assumes a web framework context (Flask or FastAPI route handlers) and adds abstractions that would conflict with PyGuard's lifespan-task pattern. The raw SDK gave us direct control over the `SocketModeClient` lifecycle, which maps cleanly onto FastAPI's `asynccontextmanager` startup/shutdown.

---

## Technical Debt Introduced or Resolved

- **Resolved:** The two-process architecture and its associated failure modes are gone. There is no bridge to restart, no WebSocket reconnect loop to maintain, and no locally bound port to conflict with.
- **Resolved:** The `websockets` dependency (constrained to `>=16.0,<17.0` to match the bridge's protocol version) is removed.
- **Introduced:** The `_to_mrkdwn()` method in `SlackService` contains a regex-based Markdown table converter. It works, but it is fragile — edge cases in table formatting (merged cells, missing separators, malformed pipes) will silently produce garbled output rather than raising an error. This is acceptable for now because agent-generated tables are rare, but it should be replaced with a proper Markdown parser before file-upload responses (F12) ship.
- **Introduced:** Slack user identity (the `sender_id` Slack user ID) is passed directly as the `user` field to `process_chat_message()`. The DB lookup tries `get_user_by_email()` then `get_user_by_name()`. Neither matches a Slack user ID, so first-time Slack users will always fail identity resolution silently. This is a known gap documented as F11 in the feature plan and must be resolved before the product goes beyond a single known user.

---

## Limitations Remaining for Future Sprints

- **No mid-execution feedback.** The `SlackService` calls `process_chat_message()` as a single blocking `await`. There is no mechanism to emit intermediate Slack messages ("Researching...", "Writing doc...") without refactoring the orchestrator to accept a callback or message-bus hook. This is the highest-impact limitation remaining.
- **No stateful conversation per thread.** Replies arrive in the correct thread, but if a user sends a follow-up message in the same thread, the `session_key` is thread-scoped but the agent has no special awareness that it is continuing a prior task. Multi-turn within a thread is supported by DB history, but the thread context is not surfaced to the orchestrator explicitly.
- **File outputs are text-only.** The data analyst sub-agent produces charts and CSVs as files on disk. The current `_send_reply()` only posts text. File upload via `files_upload_v2` is architected into the service but not wired to the agent output path yet.
