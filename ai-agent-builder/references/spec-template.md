# Agent Specification Document
**Agent Name:** {{AGENT_NAME}}
**Version:** 1.0
**Date:** {{DATE}}
**Author:** {{AUTHOR}}
**Status:** Draft

---

## Executive Summary

{{EXECUTIVE_SUMMARY}}
> _One paragraph describing what this agent does, who uses it, and what value it delivers._

---

## 1. Goal & Success Criteria

### Problem Statement
{{PROBLEM_STATEMENT}}

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
**Tier:** {{MODEL_TIER}} ({{MODEL_TIER_REASON}})

### Primary Model
**Model:** {{PRIMARY_MODEL}}
**Rationale:** {{MODEL_RATIONALE}}

### Special Requirements
{{MODEL_REQUIREMENTS}}

### Model Configuration
```yaml
model:
  primary: {{PRIMARY_MODEL}}
  tier: {{MODEL_TIER}}
  context_window: {{CONTEXT_WINDOW}}
  fallback: {{FALLBACK_MODEL}}
```

---

### Indexing & Embedding Strategy

> _Skip this section if the agent does not perform document retrieval or semantic search._

#### Embedding Model
**Model:** {{EMBEDDING_MODEL}}
**Dimension:** {{EMBEDDING_DIMENSION}}
**Rationale:** {{EMBEDDING_RATIONALE}}

#### Chunking Strategy
**Method:** {{CHUNKING_METHOD}}
**Chunk Size:** {{CHUNK_SIZE}}
**Overlap:** {{CHUNK_OVERLAP}}
**Rationale:** {{CHUNKING_RATIONALE}}

#### Vector Store
**Store:** {{VECTOR_STORE}}
**Hosting:** {{VECTOR_STORE_HOSTING}} _(managed / self-hosted / local)_
**Rationale:** {{VECTOR_STORE_RATIONALE}}

#### Retrieval Method

| Method | Enabled | Notes |
|--------|---------|-------|
| Dense (vector similarity) | {{DENSE_ENABLED}} | {{DENSE_NOTES}} |
| Sparse / BM25 | {{SPARSE_ENABLED}} | {{SPARSE_NOTES}} |
| Re-ranking | {{RERANK_ENABLED}} | {{RERANK_MODEL}} |
| HyDE | {{HYDE_ENABLED}} | {{HYDE_NOTES}} |
| Multi-query | {{MULTIQUERY_ENABLED}} | {{MULTIQUERY_NOTES}} |

**Top-k results retrieved:** {{TOP_K}}

#### Index Update Strategy
**Method:** {{INDEX_UPDATE_METHOD}}
**Schedule / Trigger:** {{INDEX_UPDATE_TRIGGER}}

#### Embedding & Index Configuration
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
  metric: {{SIMILARITY_METRIC}}  # cosine | dot_product | euclidean

retrieval:
  top_k: {{TOP_K}}
  hybrid: {{HYBRID_ENABLED}}
  reranker: {{RERANK_MODEL}}

index_updates:
  strategy: {{INDEX_UPDATE_METHOD}}
  trigger: {{INDEX_UPDATE_TRIGGER}}
```

---

## 3. Framework

### Selected Framework
**Framework:** {{FRAMEWORK}}
**Project Stage:** {{PROJECT_STAGE}}

### Rationale
{{FRAMEWORK_RATIONALE}}

### Multi-Agent Support
{{MULTI_AGENT}}

### Framework Configuration
```yaml
framework:
  name: {{FRAMEWORK}}
  version: latest
  multi_agent: {{MULTI_AGENT_BOOL}}
  observability: {{OBSERVABILITY_TOOL}}
```

---

## 4. Tools

### Tool Registry

| Tool Name | Type | Service / API | Access Level | Auth Method |
|-----------|------|--------------|--------------|-------------|
{{TOOL_TABLE_ROWS}}

### Failure Handling Strategy

| Tool | On Failure | Retry Count | Fallback |
|------|-----------|-------------|----------|
{{FAILURE_TABLE_ROWS}}

### Tool Integration Notes
{{TOOL_NOTES}}

---

## 5. Memory Architecture

### Memory Types

{{MEMORY_TYPES_TABLE}}

### Memory Configuration
```yaml
memory:
  cache:
    enabled: {{CACHE_ENABLED}}
    max_tokens: {{CACHE_MAX_TOKENS}}
  episodic:
    enabled: {{EPISODIC_ENABLED}}
    store: {{EPISODIC_STORE}}
  file_system:
    enabled: {{FS_ENABLED}}
    backend: {{FS_BACKEND}}
  shared_across_instances: {{SHARED_MEMORY}}
```

### Memory Architecture Diagram
```
┌──────────────────────────────────────────┐
│              {{AGENT_NAME}}              │
│                                          │
│  {{CACHE_BOX}}   │   {{EPISODIC_BOX}}   │
│                  │                       │
│         ┌────────┴────────┐              │
│         │ {{FS_BOX}}      │              │
│         └─────────────────┘              │
└──────────────────────────────────────────┘
```

---

## 6. Context Management

### Strategy
{{CONTEXT_STRATEGY}}

### Summarization Policy
{{SUMMARIZATION_POLICY}}

### Token Budget
**Max tokens per turn:** {{MAX_TOKENS_PER_TURN}}

### Health Metrics to Track

| Metric | Purpose | Target |
|--------|---------|--------|
{{METRICS_TABLE_ROWS}}

### Context Management Configuration
```yaml
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

## 7. Testing & Evaluation Plan

### Unit Tests

| Test ID | Test Description | Pass Criteria |
|---------|-----------------|---------------|
{{UNIT_TESTS_ROWS}}

### Edge Case Tests

| Test ID | Scenario | Expected Behavior |
|---------|---------|-------------------|
{{EDGE_TESTS_ROWS}}

### Performance Targets

| KPI | Target |
|-----|--------|
| Task Success Rate | {{SUCCESS_RATE}} |
| Max Cost per Task | {{MAX_COST}} |
| Latency (p95) | {{LATENCY_P95}} |

### Eval Strategy
{{EVAL_STRATEGY}}

---

## 8. Architecture Overview

```
┌─────────────────────────────────────────────────────────┐
│                    {{AGENT_NAME}}                        │
│                                                         │
│  ┌───────────┐    ┌──────────────┐    ┌─────────────┐  │
│  │  Input /  │───▶│  Orchestrator│───▶│   Tools     │  │
│  │  Trigger  │    │  ({{FRAMEWORK}})  │    │ {{TOOLS}}   │  │
│  └───────────┘    └──────┬───────┘    └─────────────┘  │
│                          │                              │
│                   ┌──────▼───────┐                      │
│                   │  {{PRIMARY_MODEL}}    │              │
│                   └──────┬───────┘                      │
│                          │                              │
│                   ┌──────▼───────┐                      │
│                   │   Memory     │                      │
│                   │ {{MEMORY_SUMMARY}} │                 │
│                   └──────────────┘                      │
└─────────────────────────────────────────────────────────┘
```

---

## 9. Implementation Checklist

### Phase 1 — Foundation
- [ ] Set up framework: {{FRAMEWORK}}
- [ ] Configure primary model: {{PRIMARY_MODEL}}
- [ ] Implement memory architecture
- [ ] Connect core tools

### Phase 2 — Integration
- [ ] Wire up all tools with auth and error handling
- [ ] Implement context management strategy
- [ ] Set up HITL touchpoints: {{HITL_SUMMARY}}
- [ ] Add observability / logging

### Phase 3 — Testing
- [ ] Write and run unit tests
- [ ] Execute edge case test suite
- [ ] Validate performance against targets
- [ ] Cost audit per task

### Phase 4 — Launch
- [ ] Staging environment validation
- [ ] Load / stress testing
- [ ] Documentation complete
- [ ] Monitoring dashboards live

---

## 10. Decisions Pending

{{PENDING_DECISIONS}}
> _List any open questions or decisions that need to be resolved before build begins._

---

## 11. References & Resources

- **Framework Docs:** {{FRAMEWORK_DOCS_URL}}
- **Model API Docs:** {{MODEL_DOCS_URL}}
- **Team Contacts:** {{TEAM_CONTACTS}}
- **Related Projects:** {{RELATED_PROJECTS}}

---
