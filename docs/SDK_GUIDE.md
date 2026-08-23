# Python SDK Guide (`promptmatrix-sdk`)

The official Python client SDK provides zero-dependency, low-latency integration with PromptMatrix.

---

## Installation

```bash
pip install promptmatrix-sdk
```

---

## Quickstart

```python
from promptmatrix import PromptMatrix

# Initialize client with API key and base URL (default: SaaS cloud or self-hosted)
pm = PromptMatrix(
    api_key="pm_live_...",
    base_url="https://promptmatrixx.vercel.app" # or http://localhost:8000 for local OSS
)

# Fetch prompt with runtime variable substitution
prompt_text = pm.get(
    "agent.customer_support",
    vars={"user_name": "Alex", "tier": "Enterprise"}
)

print(prompt_text)
```

---

## Async Support

```python
from promptmatrix import AsyncPromptMatrix

client = AsyncPromptMatrix(api_key="pm_live_...")
prompt = await client.get("agent.summarizer", vars={"format": "bullet_points"})
```

---

## Telemetry Feedback

Report execution metrics back to the governance dashboard:

```python
pm.feedback(
    prompt_key="agent.customer_support",
    version_id="ver_...",
    outcome="success",
    latency_ms=342,
    tokens_used=1250
)
```
