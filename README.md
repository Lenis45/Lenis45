<div align="center">

[![Typing SVG](https://readme-typing-svg.demolab.com?font=Fira+Code&size=22&pause=1000&color=58A6FF&center=true&vCenter=true&width=700&lines=AI+Engineer+%26+Automation+Architect;I+build+agent+teams+that+run+real+operations;9+agents+%C2%B7+5+departments+%C2%B7+24%2F7+on+a+Mac+Mini;send+a+goal+on+Telegram+%E2%80%94+agents+ship+the+result)](https://git.io/typing-svg)

</div>

---

My startup runs on a team of 9 Python agents I built from scratch. I send a goal on Telegram — they decompose it into tasks, route each one to a specialist (copywriter, designer, dev, researcher, reviewer), pass results between dependents via recursive CTE, and report back. I approve what ships. The rest runs on its own, 24/7, on a Mac Mini next to my desk.

That system is **[agent-os](https://github.com/Lenis45/agent-os)** — fully open source.

<div align="center">

![agents](https://img.shields.io/badge/agents-9-58a6ff?style=flat-square&labelColor=0d1117)
![departments](https://img.shields.io/badge/departments-5-58a6ff?style=flat-square&labelColor=0d1117)
![mcp tools](https://img.shields.io/badge/MCP_tools-11-58a6ff?style=flat-square&labelColor=0d1117)
![uptime](https://img.shields.io/badge/uptime-24%2F7-58a6ff?style=flat-square&labelColor=0d1117)
![launchd jobs](https://img.shields.io/badge/launchd_jobs-11-58a6ff?style=flat-square&labelColor=0d1117)

</div>

---

### How it works

```python
# Every task carries its full dependency chain —
# the reviewer gets the copywriter's exact output,
# not a summary. No context gaps between agents.

ROUTING = {
    "copywriting":  "copywriter",
    "design_brief": "designer",
    "code_task":    "dev",
    "research":     "researcher",
    "review":       "reviewer",
}
```

```mermaid
flowchart LR
    You["📱 Telegram\ngoal"] -->|1 message| E["🤖 Emilia\nOrchestrator"]
    E -->|decompose| PM["📋 Project Manager"]
    PM -->|tasks + deps| Q[("ops_db\nqueue")]
    Q --> W["Workers\n✍️ 🎨 ✅ 🔍 💻"]
    W -->|cascade output| Q
    W --> R["📊 Reports"]
    E -->|brief| CF["🏭 Content\nFactory"]
    CF -->|pending| You
```

---

### System map

```
agent-os/
├── agents/
│   ├── emilia.py              Orchestrator — receives goals, owns the big picture
│   ├── project_manager.py     Breaks goals into tasks with explicit dependencies
│   ├── worker.py              Routes tasks; runs copywriter / designer / dev / researcher / reviewer
│   ├── content_factory.py     Brief → copy → image prompt → Telegram pending approval
│   └── ops/
│       ├── email_watchdog.py  Reads Gmail, classifies, drafts replies, logs to CRM
│       ├── crm_agent.py       Syncs leads and contacts to Weeek
│       ├── calendar_agent.py  Manages schedule from natural-language instructions
│       ├── kb_curator.py      Keeps Obsidian knowledge base tidy and cross-linked
│       └── infra_monitor.py   Pings services, checks B2 backups, alerts on anomalies
│
├── mcp/server.py              FastMCP stdio — 11 tools for Claude Code / Codex / Hermes
├── dashboard/server.py        Ops panel :8099 — psycopg2 pool, /api/state in ~0.1 s
└── office-fork/               Pixel office :5070 — agents light up in real time
```

---

### Stack

**Agent layer**

![Python](https://img.shields.io/badge/Python_3.12-3776AB?logo=python&logoColor=white)
![Groq](https://img.shields.io/badge/Groq_LLaMA_3.3_70B-F55036?logoColor=white)
![Ollama](https://img.shields.io/badge/Ollama-000000?logoColor=white)
![Qdrant](https://img.shields.io/badge/Qdrant-DC244C?logoColor=white)
![FastMCP](https://img.shields.io/badge/FastMCP_stdio-6B46C1?logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?logo=redis&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL_16-4169E1?logo=postgresql&logoColor=white)

**Product backend (commercial, private)**

![Go](https://img.shields.io/badge/Go_1.26-00ADD8?logo=go&logoColor=white)
![Kafka](https://img.shields.io/badge/Kafka-231F20?logo=apachekafka&logoColor=white)
![Keycloak](https://img.shields.io/badge/Keycloak_26_RS256-4D4D4D?logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?logo=typescript&logoColor=white)
![React](https://img.shields.io/badge/React_19-61DAFB?logo=react&logoColor=black)
![Kotlin](https://img.shields.io/badge/Kotlin-7F52FF?logo=kotlin&logoColor=white)

---

### Stats

<div align="center">

<img height="170" src="https://github-readme-stats.vercel.app/api?username=Lenis45&show_icons=true&theme=github_dark&hide_border=true&count_private=true&bg_color=0d1117&title_color=58a6ff&icon_color=58a6ff&text_color=c9d1d9&rank_icon=github" />
<img height="170" src="https://github-readme-streak-stats.herokuapp.com?user=Lenis45&theme=github-dark-blue&hide_border=true&background=0d1117&ring=58a6ff&fire=58a6ff&currStreakLabel=58a6ff" />

</div>

<div align="center">

<img height="170" src="https://github-readme-stats.vercel.app/api/top-langs/?username=Lenis45&layout=compact&theme=github_dark&hide_border=true&bg_color=0d1117&title_color=58a6ff&text_color=c9d1d9&langs_count=8" />

</div>

<div align="center">

[![trophy](https://github-profile-trophy.vercel.app/?username=Lenis45&theme=algolia&no-frame=true&row=1&column=6&margin-w=8)](https://github.com/ryo-ma/github-profile-trophy)

</div>

<div align="center">

[![Activity Graph](https://github-readme-activity-graph.vercel.app/graph?username=Lenis45&theme=github-compact&bg_color=0d1117&color=58a6ff&line=58a6ff&point=ffffff&hide_border=true)](https://github.com/ashutosh00710/github-readme-activity-graph)

</div>

---

### Featured

<div align="center">

[![agent-os](https://github-readme-stats.vercel.app/api/pin/?username=Lenis45&repo=agent-os&theme=github_dark&hide_border=true&bg_color=0d1117&title_color=58a6ff&icon_color=58a6ff&text_color=c9d1d9)](https://github.com/Lenis45/agent-os)

</div>

---

<div align="center">

<img src="https://raw.githubusercontent.com/Lenis45/Lenis45/output/github-contribution-grid-snake-dark.svg?v=2" alt="contribution snake" />

</div>
