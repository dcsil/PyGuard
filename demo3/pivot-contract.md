# Pivot Contract — Sprint 3 (Slack Integration)

HYPOTHESIS: Replacing the WhatsApp channel with Slack as the primary interface will reduce required-path context switches to 0 and cut total task completion time by at least 25% compared to the Sprint 2 WhatsApp baseline (479 seconds), because Bobby already has Slack open for work and the native mrkdwn formatting eliminates the readability friction of plain-text WhatsApp responses.

KILL METRIC: If fewer than 80% of test sessions complete the full CUJ (research + Google Doc + email + calendar) via Slack DM in under 600 seconds with 0 required-path context switches, we pivot.

TRIGGER DATE: End of Sprint 3 (March 3rd, 2026).

FALLBACK OPTIONS:
1. Keep the Slack interface but scope down to a single high-value action per message (e.g., research only, or email only) to reduce execution time and increase completion rate — same channel, narrower task scope.
2. Revert to the web chat frontend as the primary interface and position Slack as an optional notification-only channel (agent sends results to Slack but accepts input only via the web UI) — different interaction model, same agent pipeline.
