# Build Trap Post-Mortem — Sprint 2

## Did We Need to Build to Learn?

### WhatsApp Channel (P0/P1 Features F1–F7)

- **Hypothesis was:** replacing the web frontend with WhatsApp would cut task completion time by 25% and eliminate required-path context switches.
- **Result:** required-path completion time dropped from 678s to ~479s (29% reduction), and context switches stayed at zero. The hypothesis held.
- **Did we need to build it?** Yes. This was capability validation — the question was "can we route the full multi-agent pipeline through WhatsApp reliably?" There was no way to answer that without actually wiring up the Baileys bridge, the WebSocket layer, and the message flow end to end. A mockup or user interview would not have told us whether Baileys handles reconnection correctly or whether the orchestrator's long-running execution blocks the async event loop.
- **What surprised us:** the Node.js bridge was essentially plug-and-play. The real integration cost was on the Python side — extracting the chat handler into a shared function so both HTTP and WhatsApp call the same pipeline. That refactor was small but would not have been obvious without building.

### Scheduling / Cron (F8-adjacent — added mid-sprint)

- **Hypothesis was:** letting the agent schedule delayed and recurring tasks would let users say things like "send this email in 5 minutes" naturally.
- **Result:** the scheduler itself worked — jobs persisted to disk, timers fired on time, responses were delivered back through WhatsApp. But the agent's use of the tool was unreliable:
  - The agent used `every_seconds=300` (recurring every 5 min) when the user said "in 5 minutes," creating an infinite loop instead of a one-time delay.
  - When the cron job fired, it ran the job's message through the full agent pipeline, which included the scheduling tools. The agent saw the message and created more cron jobs, causing exponential recursion.
  - We had to patch both issues post-testing: replaced `every_seconds` with `delay_seconds` for one-shot delays, and added a `from_cron` flag that disables scheduling tools during cron-triggered execution.
- **Did we need to build it?** Partially. We needed to build the cron service to prove the timer mechanism works. But the tool integration with the LLM-based orchestrator was where it fell apart, and that failure mode could have been predicted. We should have tested the agent's tool-calling behavior with a dry-run (no actual scheduling, just log what the agent tries to call) before wiring it into the live pipeline. That would have revealed the `every_seconds` vs one-time confusion without creating runaway jobs.
- **What we learned:** LLM tool usage is the riskiest layer. The infrastructure (timers, persistence, WebSocket delivery) was straightforward. The unpredictable part was the agent choosing the wrong parameter. Next sprint, any new tool should be tested with logged dry-runs before connecting to real side effects.

## Deferred Features — Why and What We Learned

- **F8 (progress messages) and F9 (acknowledgement):** deprioritized because WhatsApp message delivery is inherently asynchronous — there is no "typing indicator" API we control. We realized mid-sprint that implementing these requires hooks deep inside the orchestrator's sub-agent execution loop, which is more invasive than originally estimated. Still valuable, just larger than a sprint-2 scope.
- **F10 (confirmation before irreversible actions):** deprioritized because it requires pausing the orchestrator mid-execution, sending a WhatsApp message, waiting for the user's reply, and resuming. That is a fundamental change to the agent's execution model (currently fire-and-forget). We learned this is architecturally hard, not just a UX layer.
- **F12–F14 (voice, attachments, groups):** never considered for this sprint. They are new input modalities, not improvements to the existing text flow.

## What We Would Change Next Time

- **Dry-run new tools before live wiring.** The cron recursion bug cost real API calls and sent duplicate emails. A logged-only mode would have caught both the parameter confusion and the recursive loop without side effects.
- **Separate demand validation from capability validation.** For WhatsApp, building was the right call — the question was technical. For scheduling, we could have validated demand first (does Bobby actually ask for delayed tasks, or does she always want immediate execution?) before investing in the cron engine.
- **Update the pivot contract to include tool-reliability metrics.** Our kill metric only measured task completion time and context switches. It did not account for the agent misusing tools. Next sprint's contract should include a "tool accuracy" metric: percentage of tool calls where the agent selects the correct parameters.
