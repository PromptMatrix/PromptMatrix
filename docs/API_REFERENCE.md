# API Reference

Complete reference for primary PromptMatrix REST endpoints.

> Interactive OpenAPI portal with request sandbox: [promptmatrix.github.io/docs/](https://promptmatrix.github.io/docs/)

---

## 1. Hot-Serve Runtime API

### `GET /pm/serve/{prompt_key}`

Fetches the live approved prompt template for runtime execution.

- **Headers**:
  - `Authorization: Bearer <pm_live_...>` (Required)
- **Query Parameters**:
  - `format`: `text` (default) or `json`
  - `vars`: Comma-separated variable substitutions (e.g. `?vars=user=Alice,role=Admin`)
- **Response Headers**:
  - `X-PM-Version`: Current version number (e.g. `2`)
  - `X-PM-Latency`: Lookup latency in milliseconds
  - `X-Cache-Hit`: `true` or `false`
  - `X-Prompt-Key`: Resolved prompt identifier

---

## 2. Prompt Management API

### `POST /api/v1/prompts`
Create a new prompt key within an environment.

### `POST /api/v1/prompts/{prompt_id}/versions`
Submit a new draft version for a prompt.

### `POST /api/v1/prompts/{prompt_id}/promote`
Promote a prompt version from one environment (e.g. `staging`) to another (`production`). Automatically evaluates gating criteria.

### `POST /api/v1/prompts/{prompt_id}/rollback`
Rolls back a prompt to a previously approved version in 1 click.

---

## 3. Evaluation API

### `POST /api/v1/evals/{version_id}/run`
Execute automated evaluations (Rule-based or LLM-as-a-Judge via OpenAI, Anthropic, Gemini, DeepSeek, or Groq).

### `POST /pm/serve/{prompt_key}/feedback`
Report execution outcome, token counts, and latency telemetry back to PromptMatrix from SDK clients.
