# CLI Reference (`pmx`)

The `pmx` CLI manages local workspaces, promotes prompts in CI/CD pipelines, and runs local evaluations.

---

## Installation

```bash
pip install promptmatrix-ai
```

---

## Commands

### 1. Workspace Initialization
```bash
pmx init
```
Creates a local `.promptmatrix` workspace and initializes SQLite storage.

### 2. Start Local Server
```bash
pmx serve --port 8000
```
Runs the local PromptMatrix FastAPI governance server.

### 3. Evaluate Prompt
```bash
pmx eval --key agent.customer_support --content "You are a helpful support agent..."
```
Runs the 6-dimension rule-based and LLM evaluation suite locally.

### 4. Promote Prompt
```bash
pmx promote --key agent.customer_support --from staging --to production
```
Evaluates gating thresholds and promotes the prompt to production.
