# Pivot Contract — PyGuard PDF Report Delivery

HYPOTHESIS: SME executives will find the fraud intelligence report significantly more actionable when delivered as a structured PDF with embedded visualizations than as a raw Slack text message, measured by whether the report contains all required sections and at least 5 data-backed charts without placeholders.

KILL METRIC: If fewer than 80% of end-to-end report generation runs (n≥5 internal test runs) produce a verified, non-rejected PDF with all five charts embedded and all six analysis sections populated with real data (no fabricated statistics, no placeholder text), we pivot away from the LaTeX + tectonic approach.

TRIGGER DATE: End of Sprint (March 24, 2026).

FALLBACK OPTIONS:
1. Replace LaTeX + tectonic with a pure Python PDF library (reportlab or weasyprint) to eliminate the system dependency on tectonic and reduce compilation complexity while preserving chart embedding (same output format, different generation stack).
2. Drop PDF generation entirely for alpha and deliver a structured Slack Block Kit message with chart images attached as separate files — preserving the visual insight without the PDF compilation step (different delivery format, same analytical quality).
