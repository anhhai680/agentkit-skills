# Security & Guardrails Reference

Use this reference when helping a developer complete Step 9.

---

## Prompt Injection

Prompt injection is the primary attack vector for LLM agents. An attacker embeds instructions in user input or retrieved content that override the system prompt.

### Defense Strategies

**Input sanitization**
Strip or escape characters that are commonly used to embed instructions: angle brackets, backticks, curly braces used as delimiters, and phrases like "ignore previous instructions". Simple but not sufficient alone.

**Instruction hierarchy**
Design the system prompt so agent instructions clearly outrank user input. Use explicit framing:
```
SYSTEM (authoritative): You are a customer support agent. Never reveal internal data.
USER INPUT (untrusted): <user message here>
```
Some providers (Anthropic, OpenAI) have built-in instruction hierarchy via the `system` role — use it.

**LLM-based detection**
Route inputs through a lightweight classifier model before the main agent. The classifier flags likely injection attempts and can short-circuit the run. Open-source options: Rebuff, Llama Guard (prompt injection variant). Cloud options: Azure AI Content Safety, AWS Comprehend.

**All three together** is the recommended defense-in-depth posture for production agents that act on external systems.

---

## Output Guardrails

### PII Detection & Redaction

Never let agent outputs return raw PII to clients who shouldn't see it.

| Tool | Approach | Notes |
|------|----------|-------|
| Microsoft Presidio | Open-source, rule + ML | Self-hostable, good coverage |
| AWS Comprehend | Managed NLP | Scales easily, AWS-native |
| Google DLP API | Managed | Strong for GCP stacks |
| spaCy + custom rules | DIY | Full control, maintenance overhead |

Apply redaction as a post-processing step on every model output before it is returned to the caller.

### Toxicity / Harm Filtering

| Tool | Approach | Notes |
|------|----------|-------|
| Llama Guard | Open-weight LLM classifier | High accuracy, self-hostable |
| OpenAI Moderation API | Managed REST API | Free with OpenAI usage |
| Azure AI Content Safety | Managed | Multi-category (hate, violence, sexual, self-harm) |
| Perspective API (Google) | Managed | Focused on toxicity and civility |

### Hallucination / Groundedness

For RAG agents, outputs should be checkable against retrieved sources.
- **Citation enforcement**: prompt the model to cite the source chunk for every factual claim.
- **Groundedness scoring**: use a cross-encoder or dedicated model (e.g., HHEM by Vectara, SelfCheckGPT) to score whether the output is supported by the context.
- **Structured output + validation**: if output format is JSON, reject any response that doesn't conform to the schema.

---

## Action Rollback

Agents that write to external systems should have a rollback path wherever feasible.

| Pattern | How it works | Best for |
|---------|-------------|---------|
| Soft delete | Mark records as deleted, don't remove | Databases, CRM records |
| Event sourcing | Append events; revert by replaying without the bad event | Financial systems, audit-critical apps |
| Undo queue | Push every action to a queue; pop and invert on rollback | Email drafts, calendar changes, task creation |
| Two-phase commit | Confirm before committing side effects | Multi-system transactions |
| Read-only + human approval | Agent only proposes; human executes | High-stakes, low-volume actions |

When rollback is not feasible (e.g., external email already sent, payment processed), flag this explicitly in the spec's risk section and ensure HITL approval is required before the irreversible action.

---

## Audit Logging Levels

| Level | What is logged | Use when |
|-------|---------------|---------|
| Full trace | Every model call (prompt + response), every tool call (input + output), timestamps, token counts, costs | Compliance-required (HIPAA, SOC2), debugging complex failures |
| Summary | Task start, task end, outcome (success/fail), total cost, model used | Production monitoring, cost tracking |
| Errors only | Failed runs, exceptions, timeout events | Internal tools with low risk |

**Log destinations:** centralized log store (DataDog, Splunk, CloudWatch, Elasticsearch). Always include: `agent_run_id`, `user_id` or `session_id`, `timestamp`, `step_name`.

---

## Compliance Quick Reference

| Standard | Key requirements for AI agents |
|----------|-------------------------------|
| GDPR | Right to erasure (can you delete a user's memories/history?), data minimization, no PII in logs without consent, DPA with vendors |
| HIPAA | PHI must be encrypted in transit and at rest, BAA with all vendors who process PHI, audit logs mandatory, no PHI in model prompts unless vendor is HIPAA-eligible |
| SOC 2 | Access controls on agent secrets, audit logs for all data access, incident response plan required |
| PCI-DSS | Never log or pass raw card numbers through the agent; tokenize at input; quarterly vendor assessments |

---

## Secret Management Best Practices

Never hardcode API keys in agent code.

| Provider | Use when |
|----------|---------|
| Environment variables | Prototyping only — acceptable for local dev, not production |
| HashiCorp Vault | Self-hosted, multi-cloud, fine-grained access control |
| AWS Secrets Manager | AWS-native, automatic rotation, IAM integration |
| GCP Secret Manager | GCP-native, IAM-controlled |
| Azure Key Vault | Azure-native |

**Rotation policy**: set all API keys to auto-rotate every 90 days. Never share keys across environments (dev/staging/prod use separate keys).