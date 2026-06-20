<div align="center">

[![Typing SVG](https://readme-typing-svg.demolab.com?font=Fira+Code&size=22&pause=1000&color=58A6FF&center=true&vCenter=true&width=750&lines=AI+Engineer+%26+Automation+Architect;Python+%C2%B7+Go+%C2%B7+MCP+%C2%B7+Groq+%C2%B7+Qdrant+%C2%B7+Redis;9+autonomous+agents+%C2%B7+5+departments+%C2%B7+24%2F7;send+a+goal+on+Telegram+%E2%80%94+agents+ship+the+result)](https://git.io/typing-svg)

</div>

---

I design and run **autonomous AI agent systems** — teams of specialized agents that handle real operations without supervision. My current system manages a commercial product around the clock: it reads email, plans content, writes copy, coordinates tasks, syncs the CRM, and monitors infrastructure. I send a goal on Telegram. Agents do the rest.

That system is **[agent-os](https://github.com/Lenis45/agent-os)** — open source.

<div align="center">

![Python](https://img.shields.io/badge/Python_3.12-3776AB?logo=python&logoColor=white&style=flat-square)
![Go](https://img.shields.io/badge/Go_1.26-00ADD8?logo=go&logoColor=white&style=flat-square)
![Groq](https://img.shields.io/badge/Groq_LLaMA_3.3_70B-F55036?style=flat-square)
![Qdrant](https://img.shields.io/badge/Qdrant-DC244C?style=flat-square)
![Redis](https://img.shields.io/badge/Redis-DC382D?logo=redis&logoColor=white&style=flat-square)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL_16-4169E1?logo=postgresql&logoColor=white&style=flat-square)
![FastMCP](https://img.shields.io/badge/FastMCP_stdio-6B46C1?style=flat-square)

</div>

<div align="center">

![agents](https://img.shields.io/badge/agents-9-0d1117?style=flat-square&labelColor=58a6ff&color=0d1117)
![departments](https://img.shields.io/badge/departments-5-0d1117?style=flat-square&labelColor=58a6ff&color=0d1117)
![MCP tools](https://img.shields.io/badge/MCP_tools-11-0d1117?style=flat-square&labelColor=58a6ff&color=0d1117)
![launchd](https://img.shields.io/badge/launchd_jobs-11-0d1117?style=flat-square&labelColor=58a6ff&color=0d1117)
![uptime](https://img.shields.io/badge/uptime-24%2F7-0d1117?style=flat-square&labelColor=58a6ff&color=0d1117)

</div>

---

### Agent hierarchy

```mermaid
graph TD
    You["📱 You — Telegram / Dashboard"] --> E

    E["🤖 Emilia<br/>Orchestrator"] --> PM
    E --> CF

    PM["📋 Project Manager<br/>decomposes goals into tasks"] --> Q

    Q[("🗄️ ops_db<br/>task queue")] --> W

    W["⚙️ Worker<br/>router"] --> C["✍️ Copywriter"]
    W --> D["🎨 Designer"]
    W --> DV["💻 Dev"]
    W --> R["🔍 Researcher"]
    W --> RV["✅ Reviewer"]

    CF["🏭 Content Factory<br/>brief → copy → image → publish"] --> You

    E --> OPS
    OPS["🔧 Ops Department"] --> EW["📧 Email Watchdog"]
    OPS --> CRM["📊 CRM Agent"]
    OPS --> CAL["📅 Calendar"]
    OPS --> KB["🗂 KB Curator"]
    OPS --> MON["🖥 Infra Monitor"]
```

---

### How a task moves through the system

```mermaid
sequenceDiagram
    participant You as 📱 Telegram
    participant E as Emilia
    participant PM as Project Manager
    participant DB as ops_db
    participant W as Worker
    participant LLM as Groq LLaMA 3.3

    You->>E: "write a product launch post"
    E->>PM: decompose into subtasks
    PM->>DB: INSERT tasks (research → copy → review) with deps
    DB-->>W: pick up "research" (no deps, ready)
    W->>LLM: researcher prompt + context
    LLM-->>W: research output
    W->>DB: UPDATE task status=done, result=...

    DB-->>W: pick up "copy" (dep satisfied)
    W->>DB: SELECT result FROM tasks WHERE id=research_task_id
    Note over W: reviewer gets exact copywriter output<br/>via recursive CTE — zero context gap
    W->>LLM: copywriter prompt + research result
    LLM-->>W: draft copy
    W->>DB: UPDATE, cascade to reviewer

    DB-->>W: pick up "review"
    W->>LLM: reviewer prompt + full chain
    LLM-->>W: approved / edits
    W->>You: 📊 report to Telegram
```

---

### Context passing between agents — recursive CTE

Each worker fetches not just its own task but the complete output chain of all upstream dependencies. The reviewer gets the exact text the copywriter produced — no summaries, no lost context.

```sql
-- agents/db.py — fetch_task_with_context()
WITH RECURSIVE dep_chain AS (
    SELECT t.id, t.task_type, t.result, t.depends_on
    FROM tasks t
    WHERE t.id = %(task_id)s

    UNION ALL

    SELECT t.id, t.task_type, t.result, t.depends_on
    FROM tasks t
    JOIN dep_chain d ON t.id = d.depends_on
)
SELECT task_type, result
FROM dep_chain
ORDER BY id;
```

---

### Content factory pipeline

```mermaid
flowchart LR
    B["📝 Brief\n(topic, tone, platform)"] -->|Emilia sends| CF

    subgraph CF["🏭 Content Factory"]
        direction TB
        S1["1 · Researcher\ncollects facts + context"] --> S2
        S2["2 · Copywriter\nwrites post from brief + facts"] --> S3
        S3["3 · Image prompter\ngenerates DALL-E prompt"] --> S4
        S4["4 · Formatter\nadapts tone per platform"]
    end

    CF -->|pending| TG["📱 Telegram\nYou approve / reject"]
    TG -->|approved| PUB["🚀 Published\nto channel / site"]
    TG -->|rejected + note| CF
```

---

### MCP server — 11 tools for Claude Code / Codex

```python
# mcp/server.py — FastMCP stdio transport
# Claude Code connects: claude mcp add agent-os -- python mcp/server.py

@mcp.tool()
def sql_read(query: str, db: str) -> list[dict]:
    """SELECT-only. Whitelisted DBs: ops_db, customer_db. No DDL."""

@mcp.tool()
def list_tasks(status: str = "running") -> list[dict]:
    """running / queued / failed / done — full task board"""

@mcp.tool()
def list_projects() -> list[dict]: ...
def project_status(project_id: int) -> dict: ...
def recent_reports(limit: int = 10) -> list[dict]: ...
def system_status() -> dict: ...          # agent heartbeats + PIDs

def list_content(status: str) -> list[dict]: ...   # content pipeline
def create_content(brief: str) -> dict: ...
def approve_content(content_id: int) -> dict: ...
def reject_content(content_id: int, note: str) -> dict: ...

def new_project(goal: str, context: str) -> dict: ...
```

---

### Infrastructure — Mac Mini, always on

```
Mac Mini (Apple M-series, 16 GB)
│
├── launchd (11 jobs, KeepAlive=true)
│   ├── ai.emilia            Orchestrator — polls Telegram every 30 s
│   ├── ai.project_manager   Picks up new projects from ops_db
│   ├── ai.worker            Processes task queue, 4 parallel slots
│   ├── ai.content_factory   Brief → publish pipeline
│   ├── ai.email_watchdog    IMAP idle on Gmail, classifies on arrival
│   ├── ai.crm_agent         Syncs contacts to Weeek every 10 min
│   ├── ai.calendar_agent    Reads/writes Google Calendar
│   ├── ai.kb_curator        Obsidian vault — tags, links, dedup
│   ├── ai.infra_monitor     HTTP pings + B2 backup check every 5 min
│   ├── ai.dashboard         Ops panel :8099 (psycopg2 pool)
│   └── ai.office            Pixel office :5070 (React + Canvas)
│
├── PostgreSQL 16
│   ├── ops_db               tasks, projects, reports, agent_config, content
│   └── customer_db          users, leads, CRM data
│
├── Qdrant
│   ├── project_knowledge    long-term memory per project
│   └── shared_memory        cross-agent context store
│
└── Redis                    task dedup, rate-limit counters, pub/sub
```

---

### LLM routing

```python
# agents/llm.py — model is chosen per task type, not per agent
TASK_MODEL_MAP = {
    "research":     "llama-3.3-70b-versatile",   # Groq — speed + depth
    "copywriting":  "llama-3.3-70b-versatile",   # Groq
    "code_task":    "qwen2.5-coder:32b",          # Ollama local — code
    "design_brief": "gemma3:27b",                 # Ollama local — creative
    "review":       "llama-3.3-70b-versatile",   # Groq
    "ops":          "llama-3.3-70b-versatile",   # Groq
}

# Cost guard — cuts off paid API calls if monthly budget exceeded
if paid_calls_this_month >= BUDGET_LIMIT:
    model = OLLAMA_FALLBACK  # falls back to local GPU node
```

---

### Stack

**AI / Agent layer**

![Python](https://img.shields.io/badge/Python_3.12-3776AB?logo=python&logoColor=white)
![Groq](https://img.shields.io/badge/Groq_LLaMA_3.3_70B-F55036)
![Ollama](https://img.shields.io/badge/Ollama_local_GPU-000000)
![Qdrant](https://img.shields.io/badge/Qdrant-DC244C)
![Redis](https://img.shields.io/badge/Redis-DC382D?logo=redis&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL_16-4169E1?logo=postgresql&logoColor=white)
![FastMCP](https://img.shields.io/badge/FastMCP_stdio-6B46C1)
![launchd](https://img.shields.io/badge/macOS_launchd-000000?logo=apple&logoColor=white)

**Product backend (commercial · private)**

![Go](https://img.shields.io/badge/Go_1.26-00ADD8?logo=go&logoColor=white)
![Kafka](https://img.shields.io/badge/Kafka-231F20?logo=apachekafka&logoColor=white)
![Keycloak](https://img.shields.io/badge/Keycloak_26_RS256-4D4D4D)
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

<img height="150" src="https://github-readme-stats.vercel.app/api/top-langs/?username=Lenis45&layout=compact&theme=github_dark&hide_border=true&bg_color=0d1117&title_color=58a6ff&text_color=c9d1d9&langs_count=8&hide=html,css" />

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
[![online-store](https://github-readme-stats.vercel.app/api/pin/?username=Lenis45&repo=online-store&theme=github_dark&hide_border=true&bg_color=0d1117&title_color=58a6ff&icon_color=58a6ff&text_color=c9d1d9)](https://github.com/Lenis45/online-store)

</div>

<div align="center">

[![3d_portfolio](https://github-readme-stats.vercel.app/api/pin/?username=Lenis45&repo=3d_portfolio&theme=github_dark&hide_border=true&bg_color=0d1117&title_color=58a6ff&icon_color=58a6ff&text_color=c9d1d9)](https://github.com/Lenis45/3d_portfolio)
[![lenis45.github.io](https://github-readme-stats.vercel.app/api/pin/?username=Lenis45&repo=lenis45.github.io&theme=github_dark&hide_border=true&bg_color=0d1117&title_color=58a6ff&icon_color=58a6ff&text_color=c9d1d9)](https://github.com/Lenis45/lenis45.github.io)

</div>
