<div align="center">

[![Typing SVG](https://readme-typing-svg.demolab.com?font=Fira+Code&size=22&pause=1000&color=58A6FF&center=true&vCenter=true&width=650&lines=AI+Engineer+%26+Automation+Architect;Building+autonomous+agent+systems;9+agents+%C2%B7+5+departments+%C2%B7+running+24%2F7;LLM+routing+%C2%B7+MCP+%C2%B7+multi-agent+orchestration)](https://git.io/typing-svg)

</div>

---

I design and build **autonomous AI agent systems** — teams of specialized agents that handle real operations end-to-end. My current system runs 9 Python agents on a Mac Mini, manages tasks via Telegram, and operates a commercial product around the clock without me.

```python
ROUTING = {
    "copywriting":  "copywriter",
    "design_brief": "designer",
    "code_task":    "dev",
    "research":     "researcher",
    "review":       "reviewer",
}

# Tasks flow through a recursive CTE — each worker
# receives the full output chain of its dependencies.
# No context gaps between dependent steps.
```

---

### What I'm building

```
agent-os/
├── agents/            9 Python agents running under launchd
│   ├── emilia.py      Orchestrator — takes goals on Telegram, spawns projects
│   ├── project_manager.py  Decomposes goals → tasks with deps into ops_db
│   ├── worker.py      Routes tasks → copywriter / designer / dev / researcher / reviewer
│   ├── content_factory.py  Brief → copy → image prompt → publish pipeline
│   └── ops/           Email watchdog · CRM · calendar · knowledge curator · monitor
│
├── mcp/               FastMCP stdio server — 11 tools for Claude Code / Codex
│   └── server.py      SQL read · task queue · content ops · agent config · reports
│
├── dashboard/         Ops control panel at :8099 — no framework, pure psycopg2
│   └── server.py      ThreadedConnectionPool: /api/state latency 5 s → 0.1 s
│
└── office-fork/       Pixel office at :5070 — React+Canvas, agents light up live
```

---

### How a goal becomes a shipped result

```mermaid
flowchart LR
    You["📱 You\nTelegram"] -->|goal| E["🤖 Emilia\nOrchestrator"]
    E -->|decompose| PM["📋 Project Manager"]
    PM -->|tasks + deps| Q[("🗄️ ops_db\nqueue")]
    Q --> W["Workers\n✍️ 🎨 ✅ 🔍 💻"]
    W -->|cascade output| Q
    W --> R["📊 Reports hub"]
    E -->|content brief| CF["🏭 Content Factory"]
    CF -->|pending approval| You
```

---

### Stack

**AI / agent layer**

![Python](https://img.shields.io/badge/Python_3.12-3776AB?logo=python&logoColor=white)
![Groq](https://img.shields.io/badge/Groq_LLaMA_3.3_70B-F55036?logoColor=white)
![Ollama](https://img.shields.io/badge/Ollama-000000?logo=ollama&logoColor=white)
![Qdrant](https://img.shields.io/badge/Qdrant-DC244C?logoColor=white)
![FastMCP](https://img.shields.io/badge/FastMCP_stdio-6B46C1?logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL_16-4169E1?logo=postgresql&logoColor=white)

**Product backend (commercial)**

![Go](https://img.shields.io/badge/Go_1.26-00ADD8?logo=go&logoColor=white)
![Kafka](https://img.shields.io/badge/Kafka-231F20?logo=apachekafka&logoColor=white)
![Keycloak](https://img.shields.io/badge/Keycloak_26-4D4D4D?logo=keycloak&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?logo=typescript&logoColor=white)
![React](https://img.shields.io/badge/React_19-61DAFB?logo=react&logoColor=black)
![Kotlin](https://img.shields.io/badge/Kotlin-7F52FF?logo=kotlin&logoColor=white)

---

### Stats

<div align="center">

<img height="170" src="https://github-readme-stats.vercel.app/api?username=Lenis45&show_icons=true&theme=github_dark&hide_border=true&count_private=true&bg_color=0d1117&title_color=58a6ff&icon_color=58a6ff&text_color=c9d1d9" />
<img height="170" src="https://github-readme-streak-stats.herokuapp.com?user=Lenis45&theme=github-dark-blue&hide_border=true&background=0d1117&ring=58a6ff&fire=58a6ff&currStreakLabel=58a6ff" />

</div>

<div align="center">

[![trophy](https://github-profile-trophy.vercel.app/?username=Lenis45&theme=algolia&no-frame=true&row=1&column=6&margin-w=8)](https://github.com/ryo-ma/github-profile-trophy)

</div>

<div align="center">

[![Denis's Activity Graph](https://github-readme-activity-graph.vercel.app/graph?username=Lenis45&theme=github-compact&bg_color=0d1117&color=58a6ff&line=58a6ff&point=ffffff&hide_border=true)](https://github.com/ashutosh00710/github-readme-activity-graph)

</div>

---

<div align="center">

[![agent-os](https://img.shields.io/badge/▶_agent--os-open_source-58a6ff?style=for-the-badge)](https://github.com/Lenis45/agent-os)

</div>

<div align="center">

<img src="https://raw.githubusercontent.com/Lenis45/Lenis45/output/github-contribution-grid-snake-dark.svg" alt="snake animation" />

</div>
