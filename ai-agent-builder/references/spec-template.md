# Agent Specification Document
**Agent Name:** {{AGENT_NAME}}
**Version:** 1.0
**Date:** {{DATE}}
**Author:** {{AUTHOR}}
**Status:** Draft

---

## Executive Summary

{{EXECUTIVE_SUMMARY}}
> _One paragraph: what this agent does, who uses it, and what value it delivers._

---

## 1. Goal & Success Criteria

### Problem Statement
{{PROBLEM_STATEMENT}}

### Users & Trigger
**Users:** {{USER_TYPE}}
**Trigger:** {{TRIGGER_DESCRIPTION}}

### Input Modalities
{{INPUT_MODALITIES}}

### Deployment Environment
**Target:** {{DEPLOYMENT_TARGET}}
**Expected Volume:** {{DAILY_VOLUME}} runs/day

### Success Metrics

| Metric | Target | Measurement Method |
|--------|--------|--------------------|
| {{METRIC_1}} | {{TARGET_1}} | {{METHOD_1}} |
| {{METRIC_2}} | {{TARGET_2}} | {{METHOD_2}} |

### Workflow Pattern
**Selected Pattern:** {{WORKFLOW_PATTERN}}
> {{WORKFLOW_PATTERN_DESCRIPTION}}

### Human-in-the-Loop (HITL) Points
{{HITL_POINTS}}

### Hard Constraints
{{CONSTRAINTS}}

---

## 2. Model Selection

### Selected Model Tier
**Tier:** {{MODEL_TIER}} — {{MODEL_TIER_REASON}}

### Primary Model
**Model:** `{{PRIMARY_MODEL}}`
**Provider:** {{MODEL_PROVIDER}}
**Rationale:** {{MODEL_RATIONALE}}

### Fallback Model
**Model:** `{{FALLBACK_MODEL}}`
**Trigger:** {{FALLBACK_TRIGGER}}

### Special Requirements
{{MODEL_REQUIREMENTS}}

### Model Configuration
```yaml
model:
  primary: {{PRIMARY_MODEL}}
  provider: {{MODEL_PROVIDER}}
  tier: {{MODEL_TIER}}
  context_window: {{CONTEXT_WINDOW}}
  fallback: {{FALLBACK_MODEL}}
  temperature: {{TEMPERATURE}}
```

---

## 3. Embedding & Indexing Strategy

> _Skip this section if the agent does not perform document retrieval or semantic search._

### Embedding Model
**Model:** `{{EMBEDDING_MODEL}}`
**Dimension:** {{EMBEDDING_DIMENSION}}
**Rationale:** {{EMBEDDING_RATIONALE}}

### Chunking Strategy
**Method:** {{CHUNKING_METHOD}}
**Chunk Size:** {{CHUNK_SIZE}} tokens
**Overlap:** {{CHUNK_OVERLAP}} tokens
**Rationale:** {{CHUNKING_RATIONALE}}

### Vector Store
**Store:** {{VECTOR_STORE}}
**Hosting:** {{VECTOR_STORE_HOSTING}} _(managed / self-hosted / local)_
**Metadata Filters:** {{METADATA_FILTERS}}
**Rationale:** {{VECTOR_STORE_RATIONALE}}

### Retrieval Method

| Method | Enabled | Notes |
|--------|---------|-------|
| Dense (vector similarity) | {{DENSE_ENABLED}} | {{DENSE_NOTES}} |
| Sparse / BM25 | {{SPARSE_ENABLED}} | {{SPARSE_NOTES}} |
| Re-ranking | {{RERANK_ENABLED}} | {{RERANK_MODEL}} |
| HyDE | {{HYDE_ENABLED}} | — |
| Multi-query | {{MULTIQUERY_ENABLED}} | — |

**Top-k retrieved per turn:** {{TOP_K}}

### Index Update Strategy
**Method:** {{INDEX_UPDATE_METHOD}}
**Schedule / Trigger:** {{INDEX_UPDATE_TRIGGER}}

### Embedding & Index Configuration
```yaml
embedding:
  model: {{EMBEDDING_MODEL}}
  dimension: {{EMBEDDING_DIMENSION}}
  batch_size: {{EMBED_BATCH_SIZE}}

chunking:
  strategy: {{CHUNKING_METHOD}}
  chunk_size_tokens: {{CHUNK_SIZE}}
  overlap_tokens: {{CHUNK_OVERLAP}}

vector_store:
  provider: {{VECTOR_STORE}}
  index_name: {{VECTOR_INDEX_NAME}}
  metric: {{SIMILARITY_METRIC}}   # cosine | dot_product | euclidean
  metadata_filters: {{METADATA_FILTERS}}

retrieval:
  top_k: {{TOP_K}}
  hybrid: {{HYBRID_ENABLED}}
  reranker: {{RERANK_MODEL}}

index_updates:
  strategy: {{INDEX_UPDATE_METHOD}}
  trigger: {{INDEX_UPDATE_TRIGGER}}
```

---

## 4. Framework & Observability

### Selected Framework
**Framework:** {{FRAMEWORK}}
**Project Stage:** {{PROJECT_STAGE}}
**Multi-agent support:** {{MULTI_AGENT}}

### Rationale
{{FRAMEWORK_RATIONALE}}

### Observability
**Tool:** {{OBSERVABILITY_TOOL}}
**Trace granularity:** {{TRACE_GRANULARITY}}

### Framework Configuration
```yaml
framework:
  name: {{FRAMEWORK}}
  version: latest
  multi_agent: {{MULTI_AGENT_BOOL}}
  observability:
    tool: {{OBSERVABILITY_TOOL}}
    trace_level: {{TRACE_LEVEL}}   # step | turn | session
```

---

## 5. Tools

### Tool Registry

| Tool Name | Type | Service / API | Access | Auth Method | Max Calls/Run | Secrets Store |
|-----------|------|--------------|--------|-------------|---------------|---------------|
{{TOOL_TABLE_ROWS}}

### Failure Handling

| Tool | On Failure | Retry Count | Fallback |
|------|-----------|-------------|----------|
{{FAILURE_TABLE_ROWS}}

### Tool Integration Notes
{{TOOL_NOTES}}

---

## 6. Memory Architecture

### Memory Types

| Type | Enabled | Storage Backend | TTL / Eviction |
|------|---------|----------------|----------------|
| In-context / cache | {{CACHE_ENABLED}} | LLM context window | {{CACHE_MAX_TOKENS}} tokens |
| Episodic | {{EPISODIC_ENABLED}} | {{EPISODIC_STORE}} | {{EPISODIC_TTL}} |
| Semantic / vector | {{SEMANTIC_ENABLED}} | {{SEMANTIC_STORE}} | {{SEMANTIC_TTL}} |
| Procedural | {{PROCEDURAL_ENABLED}} | {{PROCEDURAL_STORE}} | {{PROCEDURAL_TTL}} |
| File system | {{FS_ENABLED}} | {{FS_BACKEND}} | {{FS_TTL}} |

### Shared Memory Across Instances
**Shared:** {{SHARED_MEMORY}}
**Write-conflict handling:** {{WRITE_CONFLICT_STRATEGY}}

### Memory Configuration
```yaml
memory:
  cache:
    enabled: {{CACHE_ENABLED}}
    max_tokens: {{CACHE_MAX_TOKENS}}
  episodic:
    enabled: {{EPISODIC_ENABLED}}
    store: {{EPISODIC_STORE}}
    ttl_days: {{EPISODIC_TTL_DAYS}}
  semantic:
    enabled: {{SEMANTIC_ENABLED}}
    store: {{SEMANTIC_STORE}}
    ttl_days: {{SEMANTIC_TTL_DAYS}}
  procedural:
    enabled: {{PROCEDURAL_ENABLED}}
    store: {{PROCEDURAL_STORE}}
  file_system:
    enabled: {{FS_ENABLED}}
    backend: {{FS_BACKEND}}
  shared_across_instances: {{SHARED_MEMORY}}
```

---

## 7. Context Management & Prompt Design

### System Prompt Structure
**Approach:** {{SYSTEM_PROMPT_STRUCTURE}}

### Prompt Engineering Approach
**Method:** {{PROMPT_ENGINEERING_METHOD}}
**Output format:** {{OUTPUT_FORMAT}}

### Context Management Strategy
{{CONTEXT_STRATEGY}}

### Summarization Policy
{{SUMMARIZATION_POLICY}}

### Token Budget
**Max tokens per turn:** {{MAX_TOKENS_PER_TURN}}

### Context Health Metrics

| Metric | Purpose | Target |
|--------|---------|--------|
{{CONTEXT_METRICS_ROWS}}

### Configuration
```yaml
prompt:
  system_prompt_structure: {{SYSTEM_PROMPT_STRUCTURE}}
  approach: {{PROMPT_ENGINEERING_METHOD}}
  output_format: {{OUTPUT_FORMAT}}

context:
  max_tokens_per_turn: {{MAX_TOKENS_PER_TURN}}
  strategy: {{CONTEXT_STRATEGY_KEY}}
  summarization:
    enabled: {{SUMMARIZE_ENABLED}}
    trigger: {{SUMMARIZE_TRIGGER}}
  rag:
    enabled: {{RAG_ENABLED}}
    retrieval_source: {{RAG_SOURCE}}
```

---

## 8. Security & Guardrails

### Sensitivity Level
**Level:** {{SENSITIVITY_LEVEL}} — {{SENSITIVITY_RATIONALE}}

### Prompt Injection Defenses

| Defense | Enabled | Notes |
|---------|---------|-------|
| Input sanitization | {{SANITIZE_ENABLED}} | {{SANITIZE_NOTES}} |
| Instruction hierarchy | {{HIERARCHY_ENABLED}} | {{HIERARCHY_NOTES}} |
| Canary tokens | {{CANARY_ENABLED}} | — |
| Separated retrieval/execution context | {{SEPARATED_CONTEXT_ENABLED}} | — |

### Output Guardrails

| Guardrail | Enabled | Tool / Method |
|-----------|---------|---------------|
| PII detection + redaction | {{PII_ENABLED}} | {{PII_TOOL}} |
| Toxicity filtering | {{TOXICITY_ENABLED}} | {{TOXICITY_TOOL}} |
| Schema validation | {{SCHEMA_VALID_ENABLED}} | {{SCHEMA_VALID_TOOL}} |
| Grounding / hallucination check | {{GROUNDING_ENABLED}} | {{GROUNDING_METHOD}} |
| Action confirmation | {{ACTION_CONFIRM_ENABLED}} | — |

### Action Rollback
**Supported:** {{ROLLBACK_SUPPORTED}}
**Mechanism:** {{ROLLBACK_MECHANISM}}

### Audit Logging

| Log Item | Retained For | Storage |
|----------|-------------|---------|
{{AUDIT_LOG_ROWS}}

### Compliance Scope
{{COMPLIANCE_REQUIREMENTS}}

### Security Configuration
```yaml
security:
  sensitivity: {{SENSITIVITY_LEVEL}}
  prompt_injection:
    input_sanitization: {{SANITIZE_ENABLED}}
    instruction_hierarchy: {{HIERARCHY_ENABLED}}
    canary_tokens: {{CANARY_ENABLED}}
  output_guardrails:
    pii_redaction: {{PII_ENABLED}}
    toxicity_filter: {{TOXICITY_ENABLED}}
    schema_validation: {{SCHEMA_VALID_ENABLED}}
    grounding_check: {{GROUNDING_ENABLED}}
  rollback: {{ROLLBACK_SUPPORTED}}
  audit_log_retention_days: {{AUDIT_LOG_RETENTION}}
  compliance: {{COMPLIANCE_LIST}}
```

---

## 9. Testing & Evaluation Plan

### Unit Tests

| Test ID | Test Description | Pass Criteria |
|---------|-----------------|---------------|
{{UNIT_TESTS_ROWS}}

### Edge Case Tests

| Test ID | Scenario | Expected Behavior |
|---------|---------|-------------------|
{{EDGE_TESTS_ROWS}}

### Adversarial / Red-team Eval
**Planned:** {{REDTEAM_PLANNED}}
**Scope:** {{REDTEAM_SCOPE}}

### Performance Targets

| KPI | Target |
|-----|--------|
| Task Success Rate | {{SUCCESS_RATE}} |
| Max Cost per Task | {{MAX_COST}} |
| Latency (p95) | {{LATENCY_P95}} |
| Max Steps per Task | {{MAX_STEPS}} |

### Eval Strategy
{{EVAL_STRATEGY}}

### Eval Dataset Storage
**Location:** {{EVAL_DATASET_LOCATION}}

---

## 10. Deployment & Operations

### Deployment Target
**Environment:** {{DEPLOYMENT_TARGET}}
**Container / Runtime:** {{CONTAINER_RUNTIME}}

### Environment Strategy
**Environments:** {{ENV_STRATEGY}}

### Rollout Strategy
**Method:** {{ROLLOUT_STRATEGY}}

### Alerting Thresholds

| Alert | Threshold | Channel |
|-------|-----------|---------|
| Error rate | >{{ERROR_RATE_THRESHOLD}}% | {{ALERT_CHANNEL}} |
| p95 latency | >{{LATENCY_THRESHOLD}}s | {{ALERT_CHANNEL}} |
| Cost per run | >${{COST_THRESHOLD}} | {{ALERT_CHANNEL}} |
| Tool failure rate | >{{TOOL_FAIL_THRESHOLD}}% | {{ALERT_CHANNEL}} |

### Incident Response
**Plan:** {{INCIDENT_PLAN}}

### Deployment Configuration
```yaml
deployment:
  target: {{DEPLOYMENT_TARGET}}
  runtime: {{CONTAINER_RUNTIME}}
  environments: {{ENV_STRATEGY}}
  rollout: {{ROLLOUT_STRATEGY}}
  alerting:
    error_rate_pct: {{ERROR_RATE_THRESHOLD}}
    latency_p95_s: {{LATENCY_THRESHOLD}}
    cost_per_run_usd: {{COST_THRESHOLD}}
  incident_response: {{INCIDENT_PLAN}}
```

---

## 11. Cost Estimate

> _All figures are rough estimates based on stated task volume and model pricing at time of writing. Actual costs will vary — verify current pricing at your provider's pricing page._

### Assumptions
{{COST_ASSUMPTIONS}}

### Per-Run Breakdown

| Component | Est. Tokens / Calls | Price per 1K tokens | Est. Cost |
|-----------|-------------------|---------------------|-----------|
| LLM input | {{AVG_INPUT_TOKENS}} tokens | ${{INPUT_PRICE_PER_1K}} | ${{INPUT_COST}} |
| LLM output | {{AVG_OUTPUT_TOKENS}} tokens | ${{OUTPUT_PRICE_PER_1K}} | ${{OUTPUT_COST}} |
| Embeddings | {{EMBEDDING_CALLS}} calls | ${{EMBED_PRICE_PER_1K}} | ${{EMBED_COST}} |
| **Total per run** | — | — | **${{COST_PER_RUN}}** |

### Projected Spend

| Period | Volume | Est. Cost |
|--------|--------|-----------|
| Per run | 1 | ${{COST_PER_RUN}} |
| Daily | {{DAILY_VOLUME}} | ${{DAILY_COST}} |
| Monthly | {{MONTHLY_VOLUME}} | ${{MONTHLY_COST}} |

### Cost Optimization Notes
{{COST_OPTIMIZATION_NOTES}}

---

## 12. Architecture Overview

```
┌──────────────────────────────────────────────────────────────────┐
│                        {{AGENT_NAME}}                            │
│                                                                  │
│  ┌─────────────┐    ┌───────────────────┐    ┌───────────────┐  │
│  │  Input /    │───▶│   Orchestrator    │───▶│  Tools        │  │
│  │  Trigger    │    │  ({{FRAMEWORK}})  │    │  {{TOOLS}}    │  │
│  └─────────────┘    └────────┬──────────┘    └───────────────┘  │
│                              │                                   │
│              ┌───────────────┼───────────────┐                  │
│              ▼               ▼               ▼                  │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐          │
│  │ {{PRIMARY_   │  │   Memory     │  │  Guardrails  │          │
│  │  MODEL}}     │  │ {{MEM_SUMM}} │  │ {{GUARD_SUMM}}│         │
│  └──────────────┘  └──────────────┘  └──────────────┘          │
│                                                                  │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │  Observability: {{OBSERVABILITY_TOOL}}                   │   │
│  └──────────────────────────────────────────────────────────┘   │
└──────────────────────────────────────────────────────────────────┘
```

---

## 13. Implementation Checklist

### Phase 1 — Foundation
- [ ] Set up framework: `{{FRAMEWORK}}`
- [ ] Configure primary model: `{{PRIMARY_MODEL}}` with fallback `{{FALLBACK_MODEL}}`
- [ ] Implement system prompt structure: {{SYSTEM_PROMPT_STRUCTURE}}
- [ ] Connect core tools with auth and secret management
- [ ] Set up observability: {{OBSERVABILITY_TOOL}}

### Phase 2 — Memory & Retrieval
- [ ] Implement memory architecture (types: {{MEMORY_TYPES_SUMMARY}})
- [ ] Set TTL / eviction policies
- [ ] If RAG: build embedding pipeline, populate vector store, validate retrieval hit rate

### Phase 3 — Security & Guardrails
- [ ] Implement prompt injection defenses
- [ ] Wire output guardrails (PII, schema validation, grounding)
- [ ] Set up audit logging with retention policy
- [ ] Verify compliance controls: {{COMPLIANCE_LIST}}
- [ ] Test rollback mechanism (if applicable)

### Phase 4 — Testing & Eval
- [ ] Write and run unit tests
- [ ] Execute edge case test suite
- [ ] Run adversarial / red-team eval: {{REDTEAM_PLANNED}}
- [ ] Validate performance targets (success rate, latency, cost)
- [ ] Establish golden dataset and store in: {{EVAL_DATASET_LOCATION}}

### Phase 5 — Deployment
- [ ] Provision environments: {{ENV_STRATEGY}}
- [ ] Configure alerting thresholds
- [ ] Execute rollout: {{ROLLOUT_STRATEGY}}
- [ ] Verify observability dashboards are live
- [ ] Confirm incident response plan with on-call team

---

## 14. Decisions Pending

{{PENDING_DECISIONS}}
> _List any open questions or decisions that need to be resolved before build begins._

---

## 15. References & Resources

- **Framework Docs:** {{FRAMEWORK_DOCS_URL}}
- **Model API Docs:** {{MODEL_DOCS_URL}}
- **Observability Docs:** {{OBSERVABILITY_DOCS_URL}}
- **Team Contacts:** {{TEAM_CONTACTS}}
- **Related Projects:** {{RELATED_PROJECTS}}

---