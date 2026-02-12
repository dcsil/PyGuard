# Release Manifest — Surface Product (A4)

## Code Repository

**Project Code Repository:** https://github.com/dcsil/PyGuard-Agentic-Agent

## What This Release Validates (Surface Product / Proof of Concept)

This release demonstrates the **Surface Product proof of concept** for an autonomous compliance follow-up agent.
**Primary interaction:** From a single instruction, the agent performs web research, generates a structured compliance report, and executes follow-up actions such as emailing the report and scheduling a meeting (end-to-end, live-deployed).

## Repository Index (Architectural Artifacts)

### 1) Proof of Concept Scope (Technical Summary)

- **File:** `proof-of-concept.md`
- Defines what “working” means for the primary interaction, lists known technical constraints/accepted limitations, and states the validation thresholds used in A4.

### 2) Baseline Critical User Journey (CUJ)

- **File:** `cuj-baseline.md`
- Includes persona + value-driven goal, journey audit table (steps, time, context switches, severity), evidence references, highlights/lowlights, product recommendations, and quantitative metrics.

### 3) Architecture & Topology Diagrams

- **System Topology Diagram (Deployment/Runtime View):** `assets/system-topology.png`
- **Architecture Diagram (Logical/Design View):** `assets/architecture-diagram.png`

These diagrams show:

- User entry point and deployed components (frontend, backend/orchestrator, external services)
- Data/request flow between components
- Decoupling boundaries that support evolution (e.g., adding transaction ingestion and fraud analysis later)

### 4) Evidence / Screenshots

- **Folder:** `assets/`
- Screenshots referenced by `cuj-baseline.md` and used as audit evidence of the current imperfect state.
