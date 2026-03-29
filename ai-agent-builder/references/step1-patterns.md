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