<div align="center">

  <img src="https://raw.githubusercontent.com/PromptMatrix/promptmatrix.github.io/main/PromptMatrix.webp" alt="PromptMatrix" width="100%">

  <h1>⬡ PromptMatrix</h1>
  <h3>The Governance Engine for AI Systems</h3>

  <p>
    <b>Stop hardcoding your LLM prompts. Start governing them.</b>
  </p>

  <p>
    <a href="https://pypi.org/project/promptmatrix-ai/"><img src="https://img.shields.io/pypi/v/promptmatrix-ai.svg?color=blue" alt="PyPI version" /></a>
    <a href="https://pypi.org/project/promptmatrix-ai/"><img src="https://img.shields.io/pypi/dm/promptmatrix-ai.svg?color=blue" alt="PyPI downloads" /></a>
    <a href="https://github.com/PromptMatrix/Promptmatrix/blob/main/LICENSE"><img src="https://img.shields.io/badge/license-MIT-green.svg" alt="MIT License" /></a>
    <a href="https://www.python.org/downloads/"><img src="https://img.shields.io/badge/python-3.11%2B-blue.svg" alt="Python 3.11+" /></a>
    <a href="https://promptmatrix.github.io/docs/"><img src="https://img.shields.io/badge/docs-scalar%20OpenAPI-purple.svg" alt="API Docs" /></a>
    <a href="https://github.com/PromptMatrix/Promptmatrix/discussions"><img src="https://img.shields.io/badge/community-discussions-brightgreen.svg" alt="Discussions" /></a>
    <a href="https://github.com/PromptMatrix/Promptmatrix/actions"><img src="https://github.com/PromptMatrix/Promptmatrix/actions/workflows/test.yml/badge.svg" alt="CI" /></a>
  </p>

  <p>
    <a href="https://promptmatrix.github.io">🌐 Website</a> •
    <a href="https://promptmatrix.github.io/docs/">📚 API Docs</a> •
    <a href="#-quick-start">Quick Start</a> •
    <a href="#-features">Features</a> •
    <a href="https://github.com/PromptMatrix/Promptmatrix/discussions">💬 Community</a> •
    <a href="https://github.com/PromptMatrix/Promptmatrix/blob/main/LICENSE">MIT License</a>
  </p>
</div>

---

**PromptMatrix** is high-performance, open-source infrastructure for AI engineering teams. It centralizes your agent prompts into a version-controlled, auditable, and evaluated registry — enabling instant updates via sub-10ms APIs without ever redeploying your codebase.

---

## ⚡️ The Core Problem

If you're building sophisticated AI agents, copilots, or internal workflows, your system prompts are currently trapped as raw strings in your repository.

When a prompt fails in production, you have to submit a PR, run CI/CD, and redeploy your entire application just to change a system instruction. **PromptMatrix fixes this.**

```python
# ❌ BEFORE: Hardcoded, ungoverned, invisible to product teams
SYSTEM_PROMPT = "You are an elite AGI-level operator. Always respond in JSON..."
agent.run(SYSTEM_PROMPT)

# ✅ AFTER: Governed, evaluated, instantly updatable
system_prompt = requests.get(
    "http://localhost:8000/pm/serve/agent.architect",
    headers={"Authorization": "Bearer pm_live_xxx"}
).text
agent.run(system_prompt)
```

---

## ✨ Features

*   **⏱️ Zero-Downtime Hot Swaps:** Update your LLM instructions in real time. Changes propagate in milliseconds.
*   **⏪ Immutable Version History:** 1-click rollbacks for broken prompts. Never lose a historical state.
*   **⚖️ Built-in LLM-As-Judge Evals:** Natively test your prompts against Anthropic, OpenAI, Google, Groq, or Mistral before deploying.
*   **🛡️ Cryptographic Security:** Eval API keys are AES-256-GCM encrypted. Integration keys are SHA-256 hashed — never stored in plaintext.
*   **🔌 Universal Serve API:** Low-latency `GET /pm/serve/{key}` with in-memory caching, variable substitution, and JSON/text output modes.
*   **📊 Visual Dashboard:** Full governance UI at `http://localhost:8000/dashboard` — vanilla JavaScript, no build step required.
*   **🔒 Zero-Dependency Eval:** Rule-based eval engine scores across 6 dimensions with zero external dependencies — works completely offline.
*   **⌨️ Full CLI:** `pmx.py` for push, pull, diff, list, eval, and promote from the terminal.
*   **🐳 Docker Ready:** Multi-stage optimized container image with non-root user execution.
*   **📱 PWA Support:** Dashboard is installable as a Progressive Web App for desktop-like local access.

---

## 🚀 Installation & Setup

PromptMatrix runs locally on SQLite with zero external database dependencies.

### Local Setup
```bash
git clone https://github.com/PromptMatrix/Promptmatrix.git
cd Promptmatrix
./start.sh        # Windows: start.bat
```
*Creates a virtual environment, installs dependencies, handles database migrations, and launches the server at `http://localhost:8000`.*

### Docker Compose
```bash
docker compose up -d
```

### Manual Setup
```bash
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
alembic upgrade head
uvicorn main:app --port 8000
```

### Cloud Deploy (Vercel)
[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https://github.com/PromptMatrix/Promptmatrix&env=JWT_SECRET_KEY,ENCRYPTION_KEY&envDescription=Generate%20secure%20keys%20with%3A%20python%20-c%20%22import%20secrets%3B%20print(secrets.token_hex(32))%22)

---


## 🏛️ Swarm Runtime Architecture

In multi-agent swarms, LLM prompts are not static text—they are **Behavioral Specifications**. PromptMatrix allows agents to query their system instructions and tool definitions dynamically at runtime, enabling hot-patching of agent swarms without code redeploys or system restarts.

```text
                    ┌─────────────────────────┐
                    │  PromptMatrix Registry  │
                    │  Persona · Tool Schema  │
                    └────────────┬────────────┘
                                 │ <5ms serve · hot-patchable
                                 ▼
    ┌────────────────────────────┴───────────────────────────┐
    │    OpenClaw / LangGraph / CrewAI Orchestrator          │
    │                                                        │
    │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐   │
    │  │ Researcher  │─▶│   Writer    │─▶│  Reviewer   │   │
    │  │ pm.serve()  │  │ pm.serve()  │  │ pm.serve()  │   │
    │  └─────────────┘  └─────────────┘  └─────────────┘   │
    └────────────────────────────────────────────────────────┘
```

---

## 🚀 Deployment Models

### 🏠 Local / Self-Hosted (This Repository)

- **Single-user, fully autonomous deployment**
- SQLite database — zero external dependencies
- Perfect for individual developers managing prompts locally
- 100% open source — MIT licensed
- Instant setup: run `./start.sh` or `start.bat`
- Login screen is skipped in development mode — dashboard opens directly

### ☁️ Team / Production (Self-Hosted PostgreSQL)

Switch to PostgreSQL for team deployments:

1. Set `DATABASE_URL=postgresql://user:password@host:5432/promptmatrix` in `.env`
2. Set `APP_ENV=production` to enable the login screen and security validators
3. Uncomment `psycopg2-binary` in `requirements.txt`
4. Run `alembic upgrade head` to apply migrations

> For multi-user team collaboration with RBAC, managed hosting, and advanced workflow features — see the [Cloud version](https://promptmatrix.github.io).

---

## ⌨️ CLI

Two ways to use the CLI:

**Globally installed (via pip):**
```bash
pip install promptmatrix-cli
pmx login
pmx push agent.system ./prompt.txt
pmx pull agent.system ./out.txt
pmx diff agent.system ./prompt.txt
pmx eval agent.system ./prompt.txt --type rule_based  # Offline — no API key needed
pmx promote agent.system production
```

**Or use the bundled script directly:**
```bash
python pmx.py status                    # Server health + version
python pmx.py list
python pmx.py push agent.system ./prompt.txt
python pmx.py eval agent.system ./prompt.txt --type rule_based
```

### CI/CD Integration

```yaml
# .github/workflows/eval_prompts.yml
- name: Evaluate prompts
  env:
    PMX_URL: ${{ secrets.PMX_URL }}
    PMX_TOKEN: ${{ secrets.PMX_TOKEN }}
  run: |
    pip install promptmatrix-cli
    pmx eval agent.system ./prompts/agent.txt --type rule_based
```

---

## 🐍 Python SDK

For integrating PromptMatrix into your Python application:

```bash
pip install promptmatrix
```

```python
from promptmatrix import PromptMatrix

pm = PromptMatrix(
    api_key="pm_live_your_key_here",
    base_url="http://localhost:8000",  # or https://promptmatrixx.vercel.app
)

# Hot-path: fetch and render a live prompt
result = pm.serve("assistant.system")
system_prompt = result.render(company="Acme", user="Alice")

# Async (FastAPI, etc.)
from promptmatrix import AsyncPromptMatrix
async with AsyncPromptMatrix(api_key="pm_live_...") as pm:
    result = await pm.serve("assistant.system")
```

SDK docs: [promptmatrix-sdk README](https://github.com/PromptMatrix/Promptmatrix/tree/main/sdk#readme)

---

## 🧪 Running Tests

```bash
source venv/bin/activate  # Windows: venv\Scripts\activate
pytest -v
```

The test suite uses an in-memory SQLite database. No external services required.

---

## 🤝 Contributing

Contributions are welcome! Please read [CONTRIBUTING.md](CONTRIBUTING.md) for project philosophy, setup steps, and test guidelines.
Bug reports and feature requests can be opened directly on the [Issues](https://github.com/PromptMatrix/Promptmatrix/issues) page.

---

## 🔒 Security

Found a vulnerability? **Do not open a public issue.**
See [SECURITY.md](SECURITY.md) for our responsible disclosure policy.

---

## 📄 License

MIT © [PromptMatrix](https://github.com/PromptMatrix/Promptmatrix)
