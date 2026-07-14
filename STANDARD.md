# The Program AI Architecture Standard

**Version 1.0 - Request for Comments** - Comment period through October 1, 2026 - [How to comment](CONTRIBUTING.md) - [License: CC BY 4.0](LICENSE.md)

*Anthony Filipovich - Latitude Consulting*

---

## 1. Why this standard exists

Organizations are deploying AI agents faster than they are governing them. 62% of organizations are experimenting with AI agents, yet in any given business function no more than 10% are scaling them, and only 39% report enterprise-level financial impact. Gartner expects over 40% of agentic AI projects to be cancelled within two years. Roughly four in five businesses are deploying or planning agents; fewer than half have any framework limiting what those agents may do unattended.

These are not model failures. They are architecture failures: numbers nobody can trace, agents nobody scoped, retrieval nobody governed, autonomy nobody defined.

This document proposes a standard - ten verifiable conformance points and the reference architecture behind them - for running an IT program office on AI. It is vendor-neutral by construction: it names storage roles, open protocols, and controls. Never products.

**What this standard is not.** Not a tool recommendation, not a maturity model you can satisfy with a slide, and not a claim that AI runs your program. Judgment, accountability, political context, and final decisions remain human. The standard makes the AI layer underneath those humans auditable, governed, and worth trusting.

**Three terms.** An **AI Pipeline** is a scheduled or event-triggered process running defined AI tasks (Extract, Classify, Score, Match, Generate) on defined data. An **AI Agent** is everything a pipeline is, plus *decide* (judgment against thresholds) and *act* (notify, route, escalate, write). The **program office** is the function coordinating multiple projects toward one strategic outcome.

## 2. The ten-point conformance test

A program office conforms when it can answer yes to all ten. Self-assessed in an afternoon; audited in a day.

| # | Conformance point |
|---|---|
| 1 | Every number an AI cites resolves to a SQL query in a system of record. |
| 2 | Every relationship claim resolves to a graph traversal - with validity intervals on the edges. |
| 3 | Every narrative claim carries provenance metadata a reader can click back to. |
| 4 | Every agent has a data contract: inputs, bundle, metadata, outputs, guardrails. |
| 5 | Every agent runs under its own least-privilege identity - no shared service accounts. |
| 6 | Every agent has a golden dataset and passed its evaluation gate before deployment. |
| 7 | Every agent action has a declared autonomy level, and promotions are eval-gated. |
| 8 | Agent telemetry is emitted in OpenTelemetry GenAI format into the observability platform you already run. |
| 9 | Data layers are exposed to agents via MCP; agent-to-agent coordination runs over A2A. |
| 10 | The controls above are mapped to ISO/IEC 42001 clauses or NIST AI RMF functions. |

Points 1-3 govern what the AI may claim. Points 4-7 govern what an agent may do. Points 8-10 make the whole thing observable and defensible to the people who fund it.

## 3. The reference architecture

![Reference architecture](assets/reference-architecture.png)

Six layers, each a distinct concern, plus cross-cutting controls at every layer:

1. **Data sources** - where program data already lives: project tools, documents, financials, communications, code and test systems, monitoring, contracts, standards.
2. **Extraction & classification** - AI pipelines that turn raw artifacts into structured, classified, sensitivity-labeled data.
3. **Storage - the Hybrid Context Stack** - Graph, SQL, and Vector roles. One physical stack per program, logically scoped per project.
4. **Intelligence** - twelve bounded agents that watch, decide, and act.
5. **Synthesis** - the cross-project reasoning layer producing the program views: critical path, risk, resources, benefits, narrative - every claim cited.
6. **Applications** - what humans consume. Applications render intelligence; the stack owns it.

## 4. Principle - The Hybrid Context Stack (points 1-2)

Pure vector RAG fails program work three predictable ways. **Numbers hallucinate** - vector retrieval is fuzzy by design; "approximately $2-3M" is not $2.34M, and executives discard AI the first time they catch a wrong number. **Relationships dissolve** - dependencies, change cascades, and traceability are graphs; embeddings approximate them. **State drifts** - "is this milestone approved?" deserves a deterministic answer, not a similarity score.

Three storage roles, each doing what it is best at:

- **Graph - relationship intelligence.** Dependencies, traceability, decision lineage, stakeholder networks. Edges are bi-temporal: every edge carries a validity interval and a source pointer; superseded edges are invalidated, never deleted - so the program can always answer "what did the dependency network look like when we approved this?"
- **SQL - operational truth.** Budgets, actuals, baselines, milestone dates, statuses, KPI values, agent signals, evaluation results. ACID, audit-ready, exact.
- **Vector - document intelligence.** Transcripts, charters, narratives, lessons learned, policy text.

These are **logical roles, not a vendor count**. Whether they run on three specialized databases or converge onto one or two platforms is a governance decision; the standard fixes responsibilities and query discipline, never products.

**The seven rules.** If it must be exact, use SQL. If it must be connected, use a graph. If it must be understood from text, use vector retrieval. If it must be explained with evidence plus relationships, combine them (GraphRAG). If it must connect, use MCP. If agents must coordinate, use A2A. If it must be done - an agent does it.

**The system-of-record rule.** Every entity declares one canonical tier. A risk is simultaneously a SQL row, a graph node, and a narrative; the other two are projections carrying provenance back to the canonical record. Pipelines are the only writers of projections, and cross-tier reconciliation runs continuously.

## 5. Principle - Provenance travels with everything (point 3)

Five metadata fields are non-negotiable on every fact, traversal, and chunk an AI touches:

1. **Provenance** - source system, document ID, ticket, or table-and-row. The reader must be able to click back to the original.
2. **Timestamp** - when generated or last modified. Every contract declares how old its inputs may be.
3. **Sensitivity label** - Public / Internal / Confidential / Restricted. Labels propagate: a summary built from Confidential inputs cannot flow to an Internal channel.
4. **Confidence** - similarity scores for vector results, validated-vs-inferred status for graph paths, data-quality status for SQL. Low confidence changes behavior: escalate, flag, or refuse.
5. **Access policy** - who may see this. The policy on an output is the most restrictive policy across its inputs.

Scope isolation is enforced, not assumed: row-level security in SQL, mandatory metadata filters on every vector query (cross-project semantic similarity is a leak channel), and node-level controls in the graph - the tier where access control is historically weakest.

## 6. Principle - Open protocols, not proprietary glue (point 9)

- **MCP (Model Context Protocol)** connects agents to data and tools. Each stack layer is exposed as an MCP server - one standard interface for any agent from any vendor.
- **A2A (Agent2Agent)** connects agents to each other: capability cards, discovery, authentication, delegation. Governed by the Linux Foundation.

A standard made of storage roles, open protocols, and controls - with zero mandatory products - is a standard any company can adopt without procurement getting a vote first.

## 7. Principle - Agents are bounded and contracted (point 4)

Every agent is a five-line statement - **purpose, scope, stack reads, allowed actions, autonomy level** - and if any of the five is missing, the agent is not production-ready. Behind the five lines sits a data contract: triggers; the exact bundle read from each tier; required metadata; outputs with reasoning chains; and guardrails.

**Reference inventory (twelve agents).** Must-have: Strategic Alignment, Connection Health, Cross-Project Constraint, Schedule Slippage, Risk Scoring & Anomaly, RAID Escalation, Executive Briefing, Performance & Variance. Nice-to-have: Regulatory Monitoring, Resource Conflict, Vendor Performance & SLA, Leading Indicators.

Two design rules: agents that read but never write are pipelines misnamed as agents; and intelligence emerges from how narrow agents are wired together - never from making one agent omniscient.

## 8. Principle - Every agent is an identity (point 5)

Classic IAM knows humans and service accounts; agents are neither. Every agent runs under its own first-class non-human identity: its own credentials, just-in-time scoped tokens matched to its data contract, lifecycle management, revocation at retirement. This turns "bounded actions" from an aspiration into an enforced property. Stepping stone for early adopters: one dedicated, scoped service account per agent - never shared, never borrowed from a human.

## 9. Principle - The autonomy ladder (point 7)

Every agent action is assigned one of four levels, recorded in its contract:

| Level | Meaning |
|---|---|
| **L1 - Recommend** | The agent drafts; a human executes. Day-one default. |
| **L2 - Act with approval** | The agent prepares the action in full; a human approves before it fires. |
| **L3 - Act with notification** | The agent executes; the accountable human is informed and can reverse. |
| **L4 - Act autonomously** | The agent executes within bounded scope; the audit trail is the oversight. |

Promotion is **earned, not configured** - an action moves up only after passing its evaluation gates over a declared period, and some actions are capped permanently. The ladder answers the only question executives actually ask: *what can the AI do without asking?*

## 10. Principle - The evaluation harness (point 6)

The harness is to agents what UAT is to software. Four elements: a **golden dataset** per agent (real inputs with known-correct outputs: happy paths, edge cases, adversarial cases); a **deployment gate** (evals gate initial deployment, every model or prompt change, and every autonomy promotion); a **regression cadence** (full re-runs at least weekly to catch drift); and **LLM-as-judge** for generated narratives, human-sampled for calibration.

## 11. Principle - Observable, breakered, and governed (points 8, 10)

Every agent action is logged with agent identity, trigger conditions, and inputs - emitted in **OpenTelemetry GenAI** format, riding the observability platform the enterprise already runs. Multi-agent chains get circuit breakers: maximum chain depth, per-item cooldowns, idempotent actions.

The control set maps onto **ISO/IEC 42001** and the **NIST AI RMF** functions (Govern, Map, Measure, Manage). Neither framework was designed for autonomous agents; the ladder and the harness are this standard's extension where formal standards are still catching up.

## 12. Adoption - Good, Better, Best

- **Good (months 1-3).** The SQL system of record you already have plus lightweight vector search over documents. One scoped service account per AI process. Start the golden datasets.
- **Better (months 4-9).** Add the dependency graph (relational tables are a fine start), the first contracted agents at L1-L2, MCP interfaces, and eval gates on everything that ships.
- **Best (months 12-24).** Full stack at both scopes, the twelve-agent inventory, per-agent identity, A2A coordination, OpenTelemetry GenAI end to end, and the ten-point test passing an external audit.

Each level is a defensible stopping point. The ten-point test tells you exactly where you are - that is what makes this a standard rather than an aspiration.

## 13. Sources

McKinsey, *The State of AI* (2025) and *State of AI Trust in 2026* - Gartner agentic-AI forecasts (2025-2026) - Anthropic, *Building Effective Agents* (2024) - Linux Foundation, A2A Protocol (2025-2026) - Model Context Protocol (modelcontextprotocol.io) - OpenTelemetry GenAI semantic conventions (2026) - Rasmussen et al., *Zep: A Temporal Knowledge Graph Architecture for Agent Memory*, arXiv:2501.13956 - ISO/IEC 42001 - NIST AI Risk Management Framework - EU AI Act implementation timeline (artificialintelligenceact.eu).

---

*Stop using AI randomly. Start thinking in systems.*
