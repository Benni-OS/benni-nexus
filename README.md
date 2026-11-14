<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0D0D0D,50:302b63,100:7000FF&height=200&section=header&text=benni-nexus&fontSize=72&fontColor=ffffff&fontAlignY=38&desc=LLM%20Gateway%20for%20the%20Benni%20OS%20Ecosystem%20%E2%80%94%20Route.%20Balance.%20Observe.&descAlignY=58&descSize=16&animation=fadeIn" width="100%"/>

<br/>

<img src="https://readme-typing-svg.demolab.com?font=JetBrains+Mono&weight=700&size=20&duration=3000&pause=800&color=7000FF&center=true&vCenter=true&multiline=true&repeat=true&width=800&height=80&lines=One+Endpoint.+All+Your+LLMs.+Zero+Hardcoding.;cheap-first+%E2%80%A2+fast-first+%E2%80%A2+quality-first+%E2%80%A2+failover;OpenAI-Compatible+%E2%80%94+Drop-In+for+Any+SDK" alt="Typing SVG" />

<br/><br/>

[![npm](https://img.shields.io/npm/v/benni-nexus?style=for-the-badge&logo=npm&logoColor=white&color=7000FF)](https://www.npmjs.com/package/benni-nexus)
[![CI](https://img.shields.io/github/actions/workflow/status/benni-os/benni-nexus/ci.yml?branch=main&style=for-the-badge&logo=githubactions&logoColor=white&label=CI)](https://github.com/benni-os/benni-nexus/actions)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.x-00B0FF?style=for-the-badge&logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![Fastify](https://img.shields.io/badge/Fastify-v5-FF007A?style=for-the-badge&logo=fastify&logoColor=white)](https://fastify.dev)
[![License: MIT](https://img.shields.io/badge/License-MIT-00C853?style=for-the-badge&logo=opensourceinitiative&logoColor=white)](LICENSE)
[![Benni OS](https://img.shields.io/badge/Benni%20OS-benni--os.net-0D0D0D?style=for-the-badge)](https://benni-os.net)

<br/>

> **"One endpoint. All your LLMs. Zero hardcoding."**

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png" width="100%"/>

<br/>

<img src="assets/demo.gif" alt="Benni Nexus Intelligent LLM Router Demo" width="100%"/>

<br/>

</div>

<br/>

## ⭐ What Is benni-nexus?

**benni-nexus** is the intelligent LLM gateway of the **[Benni OS](https://github.com/benni-os)** ecosystem — a single OpenAI-compatible endpoint that routes, balances, and observes all your LLM backends automatically.

Every production AI stack ends up hardcoding different backends: local Ollama for dev, Groq for speed, OpenAI for quality. `benni-nexus` puts a single intelligent router in front of all of them.

```
Your App / Any SDK (OpenAI, LangChain, LlamaIndex, Vercel AI...)
         ↓ POST /v1/chat/completions (OpenAI-compatible)
    ┌─────────────────────────┐
    │      benni-nexus           │
    │   ┌────────────┐          │
    │   │   Router    │          │
    │   └─────┬─────┘          │
    └──────────│────────────┘
         ┬─────────┬────────┬
    Ollama   Groq    OpenAI   Gemini
   (local)  (fast)  (quality) (multi)
```

<br/>

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/dark.png" width="100%"/>

## ⚡ Core Doctrine

> These are not settings. These are architecture laws.

| ☔ Principle | 🔧 Implementation |
|---|---|
| **Single Endpoint** | One `POST /v1/chat/completions` — every backend behind it, no app rewrites |
| **Strategy-Driven Routing** | `cheap-first` · `fast-first` · `quality-first` · `round-robin` · `failover` |
| **OpenAI-Compatible** | Drop-in for any SDK — LangChain, LlamaIndex, Vercel AI, AutoGen, CrewAI |
| **Local-First Option** | Ollama as first-class backend — zero cloud required for dev |
| **Observable by Default** | Prometheus metrics + per-backend health checks (v0.2 roadmap) |
| **Zero Lock-In** | Add or remove backends via config — no code changes, no redeployment |

<br/>

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/dark.png" width="100%"/>

## 🚀 Quick Start

```bash
# Install globally
npm install -g benni-nexus

# Generate config
nexus init

# Start the gateway
nexus start
# ⚡ benni-nexus running on http://localhost:4000
```

Now **any** OpenAI-compatible client works out of the box:

```typescript
import OpenAI from 'openai';

const client = new OpenAI({
  baseURL: 'http://localhost:4000/v1',
  apiKey: 'nexus', // any string
});

const res = await client.chat.completions.create({
  model: 'auto', // benni-nexus picks the best backend
  messages: [{ role: 'user', content: 'Explain MCP in one paragraph.' }],
});
```

<br/>

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/dark.png" width="100%"/>

## 🎯 Routing Strategies

| Strategy | Description | Best For |
|---|---|---|
| `cheap-first` | Local models first, cloud as fallback | Dev environments, cost-sensitive workloads |
| `fast-first` | Routes to lowest measured P50 latency | Real-time apps, chat UIs |
| `quality-first` | Routes to highest quality-scored backend | Production pipelines, summarization |
| `round-robin` | Even distribution across all backends | Load testing, equal-quality backends |
| `failover` | Primary with automatic fallback chain | High-availability production |

<br/>

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/dark.png" width="100%"/>

## ⚙️ Configuration

```json
{
  "port": 4000,
  "strategy": "cheap-first",
  "backends": [
    { "name": "ollama", "type": "ollama", "url": "http://localhost:11434", "models": ["llama3.1:8b", "mistral:7b"] },
    { "name": "groq",   "type": "groq",   "apiKey": "$GROQ_API_KEY" },
    { "name": "openai", "type": "openai", "apiKey": "$OPENAI_API_KEY" },
    { "name": "gemini", "type": "gemini", "apiKey": "$GEMINI_API_KEY" }
  ]
}
```

<br/>

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/dark.png" width="100%"/>

## 🔌 Supported Backends

| Backend | Type | Notes |
|---|---|---|
| **Ollama** | `ollama` | Local models — Llama, Mistral, Gemma, Phi, Qwen... |
| **Groq** | `groq` | Ultra-fast inference on Llama, Mixtral |
| **OpenAI** | `openai` | GPT-4o, GPT-4o-mini, o1... |
| **Google Gemini** | `gemini` | Gemini 2.0 Flash, Pro |
| **Anthropic** *(v0.3)* | `anthropic` | Claude 3.5, Claude 4... |
| **Custom** *(v0.3)* | `custom` | Any OpenAI-compatible endpoint |

<br/>

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/dark.png" width="100%"/>

## 📊 Observability *(v0.2 roadmap)*

```
GET /metrics

nexus_requests_total{backend="ollama",status="success"} 1420
nexus_requests_total{backend="groq",status="success"} 380
nexus_latency_p50_ms{backend="ollama"} 142
nexus_latency_p50_ms{backend="groq"} 38
```

Prometheus metrics + per-backend health checks + live dashboard — coming in v0.2.

<br/>

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/dark.png" width="100%"/>

## 🛠️ Development

```bash
git clone https://github.com/benni-os/benni-nexus
cd benni-nexus
npm install
npm run dev       # ts-node live reload
npm test          # vitest
npm run typecheck # tsc --noEmit
```

<br/>

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/dark.png" width="100%"/>

## 🤝 Contributing

All contributions welcome. Check [good first issues](https://github.com/benni-os/benni-nexus/labels/good%20first%20issue) for beginner-friendly tasks.
See [CONTRIBUTING.md](CONTRIBUTING.md) for full guidelines.

<br/>

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/dark.png" width="100%"/>

## 🌐 The Benni OS Open-Source Ecosystem

`benni-nexus` is part of the coordinated **Benni Master OS v9.4** open-source infrastructure suite:

| Repository | Focus & Architecture | Stars |
| :--- | :--- | :---: |
| **[`conductor-sdd`](https://github.com/Benni-OS/conductor-sdd)** | Zero-Dependency Spec-Driven Development, Static AST Drift Detector & MCP Guardrail | [![GitHub Stars](https://img.shields.io/github/stars/Benni-OS/conductor-sdd?style=flat)](https://github.com/Benni-OS/conductor-sdd) |
| **[`benni-inference-engine`](https://github.com/Benni-OS/benni-inference-engine)** | High-Throughput Sovereign Local Inference for 8GB–16GB Consumer GPUs | [![GitHub Stars](https://img.shields.io/github/stars/Benni-OS/benni-inference-engine?style=flat)](https://github.com/Benni-OS/benni-inference-engine) |
| **[`agent-eval-harness`](https://github.com/BenniAlencar/agent-eval-harness)** | Deterministic Zero-Cloud Agent Evaluation, Red-Teaming & Benchmark Harness | [![GitHub Stars](https://img.shields.io/github/stars/BenniAlencar/agent-eval-harness?style=flat)](https://github.com/BenniAlencar/agent-eval-harness) |
| **[`agent-code-sandbox`](https://github.com/BenniAlencar/agent-code-sandbox)** | Micro-Second Zero-Dependency Code Sandbox & Execution Shield for AI Agents | [![GitHub Stars](https://img.shields.io/github/stars/BenniAlencar/agent-code-sandbox?style=flat)](https://github.com/BenniAlencar/agent-code-sandbox) |
| **[`agent-deep-research`](https://github.com/BenniAlencar/agent-deep-research)** | Autonomous Multi-Source Deep Research & Evidence Synthesis Engine | [![GitHub Stars](https://img.shields.io/github/stars/BenniAlencar/agent-deep-research?style=flat)](https://github.com/BenniAlencar/agent-deep-research) |
| **[`agent-browser-mesh`](https://github.com/BenniAlencar/agent-browser-mesh)** | Token-Saving Resilient Headless Browser Automation & A11y DOM Compiler | [![GitHub Stars](https://img.shields.io/github/stars/BenniAlencar/agent-browser-mesh?style=flat)](https://github.com/BenniAlencar/agent-browser-mesh) |
| **[`agent-intent-compiler`](https://github.com/BenniAlencar/agent-intent-compiler)** | Deterministic Intent Compiler & Typed DAG Execution Planner for AI Agents | [![GitHub Stars](https://img.shields.io/github/stars/BenniAlencar/agent-intent-compiler?style=flat)](https://github.com/BenniAlencar/agent-intent-compiler) |
| **[`agent-token-optimizer`](https://github.com/BenniAlencar/agent-token-optimizer)** | Sub-Millisecond Context Compression & Semantic Deduplication Engine | [![GitHub Stars](https://img.shields.io/github/stars/BenniAlencar/agent-token-optimizer?style=flat)](https://github.com/BenniAlencar/agent-token-optimizer) |
| **[`bio-sync-operator-os`](https://github.com/BenniAlencar/bio-sync-operator-os)** | Real-Time Biometric Swarm Governance & 120 FPS Resonant Kinetic Interface | [![GitHub Stars](https://img.shields.io/github/stars/BenniAlencar/bio-sync-operator-os?style=flat)](https://github.com/BenniAlencar/bio-sync-operator-os) |
| **[`agent-cyber-sentinel`](https://github.com/BenniAlencar/agent-cyber-sentinel)** | Zero-Trust Runtime Security, Prompt Injection Firewall & Secrets Redaction | [![GitHub Stars](https://img.shields.io/github/stars/BenniAlencar/agent-cyber-sentinel?style=flat)](https://github.com/BenniAlencar/agent-cyber-sentinel) |
| **[`agent-run-ledger`](https://github.com/BenniAlencar/agent-run-ledger)** | Deterministic Multi-Step Task Execution Ledger & Anti-Looping Engine | [![GitHub Stars](https://img.shields.io/github/stars/BenniAlencar/agent-run-ledger?style=flat)](https://github.com/BenniAlencar/agent-run-ledger) |
| **[`reality-lock`](https://github.com/BenniAlencar/reality-lock)** | Cryptographic State Verification & Invariant Proof Engine for Agents | [![GitHub Stars](https://img.shields.io/github/stars/BenniAlencar/reality-lock?style=flat)](https://github.com/BenniAlencar/reality-lock) |
| **[`agent-memory-mesh`](https://github.com/BenniAlencar/agent-memory-mesh)** | Hybrid Vector + BM25 Hierarchical Session Memory & Continuity Engine | [![GitHub Stars](https://img.shields.io/github/stars/BenniAlencar/agent-memory-mesh?style=flat)](https://github.com/BenniAlencar/agent-memory-mesh) |
| **[`benni-operator-gateway`](https://github.com/Benni-OS/benni-operator-gateway)** | Human-in-the-Loop Approval & Policy Enforcement Gateway | [![GitHub Stars](https://img.shields.io/github/stars/Benni-OS/benni-operator-gateway?style=flat)](https://github.com/Benni-OS/benni-operator-gateway) |
| **[`mcp-forge`](https://github.com/Benni-OS/mcp-forge)** | Rapid Model Context Protocol (MCP) Server Scaffolding & Dynamic Tooling | [![GitHub Stars](https://img.shields.io/github/stars/Benni-OS/mcp-forge?style=flat)](https://github.com/Benni-OS/mcp-forge) |
| **[`benni-nexus`](https://github.com/Benni-OS/benni-nexus)** | Autonomous Multi-Agent Intent Routing & Swarm Coordination Hub | [![GitHub Stars](https://img.shields.io/github/stars/Benni-OS/benni-nexus?style=flat)](https://github.com/Benni-OS/benni-nexus) |
| **[`community-autopilot`](https://github.com/Benni-OS/community-autopilot)** | Autonomous GitHub Community Triaging & PR Feedback Agent | [![GitHub Stars](https://img.shields.io/github/stars/Benni-OS/community-autopilot?style=flat)](https://github.com/Benni-OS/community-autopilot) |

---

## 🏛️ Commercial & Sovereign Architecture

Need autonomous multi-agent swarm orchestration, private datacenter infrastructure, or enterprise governance?

- **Official Website:** [https://benni-os.net](https://benni-os.net)
- **Operator Mode Playbook:** [https://benni-os.net/blog/academy.html](https://benni-os.net/blog/academy.html)
- **GitHub Organization:** [https://github.com/Benni-OS](https://github.com/Benni-OS)
- **Maintainer:** [Benni Alencar](https://github.com/BenniAlencar)

---

## 📄 License & Provenance

- **License:** [MIT License](LICENSE)
- **Author:** Benni Alencar (`bennialencar@benni-os.net`)
- **Architecture:** Benni Master OS v9.4 Tier-0 Governance Standard

<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:7000FF,50:302b63,100:0D0D0D&height=120&section=footer" width="100%"/>

**benni-nexus** — *Open-Source LLM Gateway by [Benni OS](https://github.com/Benni-OS)* • [benni-os.net](https://benni-os.net) • `MIT License`

`SINGLE_ENDPOINT` • `STRATEGY_ROUTING` • `OPENAI_COMPATIBLE` • `LOCAL_FIRST` • `MIT_LICENSE`

Engineered by **Benni Alencar** and the Benni OS Core Engineering Group

</div>
