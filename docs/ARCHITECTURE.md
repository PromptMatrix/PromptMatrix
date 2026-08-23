# Architecture & System Design

PromptMatrix separates the **Runtime Serving Path** from the **Governance & Management Path**.

---

## 1. Core Principles

1. **Sub-10ms Runtime Latency**: Agents call `GET /pm/serve/{key}` at high frequency. The runtime path uses a multi-tier cache (Redis / In-memory) and non-blocking asynchronous I/O.
2. **Immutable Versioning**: Prompt edits never overwrite existing versions. Every save creates an incremented version number (`v1`, `v2`, `v3`) with cryptographic audit logs.
3. **Automated Evaluation Gates**: Prompts must pass automated evaluations (rule-based heuristics + LLM-as-a-judge scoring) before being promoted to `production`.
4. **Zero Downtime Updates**: Changing a prompt in production is an atomic pointer update (`live_version_id`), taking effect instantly across all connected agents without redeployment.

---

## 2. Multi-Environment Promotion Lifecycle

```
[Development] ────────> [Staging] ────────> [Production]
      │                     │                     │
      ▼                     ▼                     ▼
 Draft Prompt       LLM Eval Scoring      Hot-Serve Cache
 (Ad-hoc edits)     (Pass threshold >= 7.0) (Sub-10ms SLA)
```

---

## 3. Storage & Persistence Models

- **Organisation**: Multi-tenant isolation partition with RBAC policies.
- **Project**: Logical grouping of prompts for an application or swarm.
- **Environment**: Target stages (`dev`, `staging`, `prod`) with independent API keys and eval thresholds.
- **Prompt & PromptVersion**: Prompts contain metadata and reference a `live_version_id`. `PromptVersion` stores the raw text template, required variables, and author diffs.
- **PromptEval**: Records historical evaluation scores, latencies, token consumption, and judge rationale.
- **AuditLog**: Tamper-evident record of all workspace modifications.
