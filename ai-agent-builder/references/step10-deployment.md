# Deployment & Operations Reference

Use this reference when helping a developer complete Step 10.

---

## Hosting Target Decision Guide

| Hosting | Best For | Trade-offs |
|---------|---------|-----------|
| Serverless (Lambda, Cloud Run) | Event-driven, bursty, low baseline traffic | Cold start latency; stateless only; max execution time limits (15min Lambda) |
| Managed Kubernetes (EKS, GKE, AKS) | High-throughput, stateful, always-on agents | Higher ops overhead; more control; auto-scaling |
| Self-managed VMs | On-prem requirement, strict data sovereignty | Full control; you manage patching and scaling |
| Edge (Cloudflare Workers, Fastly) | Ultra-low latency, geographically distributed | Limited runtime (CPU time caps); no long-running tasks |
| Embedded in existing backend | Simple integration, low volume | Couples agent lifecycle to host service; harder to scale independently |

---

## Rollout Strategy Guide

| Strategy | How it works | Best for |
|----------|-------------|---------|
| Full deploy | All traffic switches immediately after deploy | Low-risk, internal tools, well-tested agents |
| Canary | Route 1–5% of traffic to new version; monitor; ramp gradually | Production agents with real user impact |
| Blue-green | Two identical environments; switch DNS/load balancer | Zero-downtime deploys; instant rollback |
| Feature flag | Enable new version per user/tenant via flag | Gradual rollout with fine-grained control; A/B testing |

**Recommendation for agentic AI specifically**: always use canary or feature flag rollout. Agent behavior changes can be subtle and only appear at scale — you want the ability to catch regressions before full exposure.

---

## Alerting Thresholds: Recommended Starting Points

These are starting points — tune to your agent's observed baseline.

| Signal | Starting Threshold | Notes |
|--------|-------------------|-------|
| Error rate | > 1% over 5 minutes | Tighten to 0.1% for high-stakes agents |
| Latency p95 | > 2× baseline | Set baseline in staging load test |
| Cost per hour | > 120% of projected | Catch runaway loops early |
| Tool failure rate | > 5% | Individual tool failure is often transient; sustained rate is a signal |
| Token usage per run | > 2× expected | Signals prompt injection or infinite loops |

---

## Kill Switch Patterns

Every production agent should have a way to stop it without a full deployment.

| Pattern | Implementation | Notes |
|---------|---------------|-------|
| Feature flag off | Set flag to false; agent returns graceful error | Fastest; requires flag infra |
| Database circuit breaker | Agent checks a "enabled" flag in DB before each run | Simple; DB is a dependency |
| API gateway throttle | Drop all traffic at the gateway | No code changes; blunt instrument |
| Queue pause | Pause the input queue; drain in-flight runs | Best for async/queue-based agents |

---

## CI/CD Pipeline Recommended Gates for AI Agents

Standard software gates plus agent-specific ones:

1. Unit tests (tool schema validation, fallback triggers)
2. Prompt regression tests (golden dataset, LLM-as-judge score ≥ threshold)
3. Adversarial prompt test suite
4. Cost-per-run check (fail if estimated cost exceeds budget × 1.5)
5. Security scan (secrets not in code, dependency vulnerabilities)
6. Staging deploy + smoke test
7. Canary deploy + 15-minute monitoring window
8. Full deploy

---

## Observability Tool Comparison

| Tool | Best For | Key Features |
|------|---------|-------------|
| LangSmith | LangChain stacks | Native tracing, dataset management, prompt hub |
| Arize Phoenix | Model-agnostic, OSS | Traces, embeddings, drift detection, self-hostable |
| Helicone | Lightweight proxy logging | Drop-in for OpenAI/Anthropic, cost tracking, rate limiting |
| OpenTelemetry (custom) | Multi-language, existing OTel infra | Full control; requires more setup |
| Provider dashboards | Quick start, provider-native | Limited cross-provider visibility |

**Minimum viable observability for production:**
- Per-run: run ID, model, input/output token count, latency, cost, outcome (success/fail)
- Per-step: tool name, input, output, latency, error (if any)
- Aggregated: daily cost, p50/p95/p99 latency, error rate, task success rate