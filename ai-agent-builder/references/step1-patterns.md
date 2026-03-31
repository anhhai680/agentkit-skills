# Workflow Design Patterns Reference

Use this reference when helping a developer choose the right workflow pattern in Step 1.

## Pattern Descriptions

### A) Sequential
Tasks execute one after another in a fixed, predictable order. Each step's output is the next step's input.

**Best for:** Linear pipelines, document processing, data transformation chains, report generation.
**Example:** User sends a support ticket → classify intent → retrieve relevant docs → draft response → send reply.
**Tradeoff:** Simple to debug, but inflexible. Any step failure halts the whole pipeline.

### B) Parallel
Multiple tasks run concurrently and their results are merged or aggregated.

**Best for:** Research aggregation, multi-source data collection, tasks where sub-tasks are independent.
**Example:** Research agent fans out to 5 sources simultaneously, then synthesizes results.
**Tradeoff:** Faster than sequential, but requires merging logic and more complex error handling.

### C) Loop / Retry
The agent repeats a process until a condition is satisfied (a goal is met, quality threshold passed, or max attempts reached).

**Best for:** Agentic coding (run → test → fix → repeat), iterative refinement, quality-gated outputs.
**Example:** Code-writing agent generates code → runs tests → fixes errors → loops until tests pass or N attempts are exceeded.
**Tradeoff:** Powerful for open-ended tasks, but needs a hard stop condition to prevent infinite loops.

### D) Hierarchical (Orchestrator + Sub-agents)
An orchestrator agent breaks down the goal and delegates subtasks to specialized sub-agents, then aggregates their results.

**Best for:** Complex multi-step tasks, tasks requiring diverse expertise, large-scale automation.
**Example:** Project manager agent delegates: research sub-agent, writer sub-agent, reviewer sub-agent.
**Tradeoff:** Most powerful pattern, but highest complexity. Sub-agents need clear interfaces and error propagation.

### E) Event-Driven
The agent is idle until a trigger fires — then activates, completes its task, and returns to idle.

**Best for:** Reactive systems, integration workflows, background processors, webhooks, scheduled tasks.
**Example:** Agent wakes when a new GitHub PR is opened, reviews it, and posts a comment.
**Tradeoff:** Efficient (no wasted compute), but harder to test and debug since execution is asynchronous.

### F) Hybrid / Composite
Real-world agents often combine patterns. An orchestrator (Hierarchical) may fan out to parallel sub-agents, each running a sequential pipeline internally, with retry loops on quality gates.

**Best for:** Complex production systems with multiple stages, mixed latency requirements, and diverse sub-tasks.
**Example:** A research agent orchestrates three parallel sub-agents (web search, DB query, doc retrieval). Each sub-agent runs a sequential pipeline internally. The orchestrator retries any sub-agent that returns low-confidence results before synthesizing the final answer.
**Tradeoff:** Most expressive, but highest design and debugging complexity. Only reach for this when simpler patterns clearly won't cover the requirements.

---

## Choosing a Pattern

| Scenario | Recommended Pattern |
|---------|-------------------|
| Fixed sequence of steps, predictable inputs | Sequential |
| Multiple independent data sources / tasks | Parallel |
| Open-ended task needing quality validation | Loop / Retry |
| Complex goal needing specialization | Hierarchical |
| React to external triggers / events | Event-Driven |
| Long-running workflow with multiple phases | Hierarchical + Sequential |
| Enterprise agent with mixed sub-task types | Hybrid / Composite |

## Input / Output Modality Notes

The input and output modalities affect architecture decisions:

| Modality | Implication |
|----------|------------|
| Voice / audio input | Need ASR (speech-to-text) stage before the model |
| Image / screenshot | Need a vision-capable model (see Step 2) |
| Structured data (JSON, CSV) | Consider schema validation at ingestion |
| File / document | Need a parsing + chunking pipeline (see Step 3) |
| API event / webhook | Event-driven pattern is strongly recommended |
| Action on external system | Security & rollback become critical (see Step 9) |

## Volume / Scale Notes

Task volume shapes infrastructure choices across multiple steps:

| Volume | Framework implication | Memory implication | Cost implication |
|--------|----------------------|-------------------|-----------------|
| Low (<100/day) | Prototype frameworks OK | In-process memory fine | Cost is negligible; optimize later |
| Medium (100–10K/day) | Production framework needed | Shared external store | Monitor cost per task actively |
| High (10K–1M/day) | Async queue + horizontal scale | Distributed shared memory | Cost dominates; aggressive optimization required |
| Bursty | Serverless / auto-scaling target | Stateless preferred | Pre-warm or accept cold-start latency |