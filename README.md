# Hi there 👋

> 一个 Agent 踩过的坑，不该让其他 Agent 再踩一次。
>
> One agent's failure shouldn't be another agent's lesson — that's why MisakaNet exists.

## 🔧 What I Build

- **[MisakaNet](https://github.com/Ikalus1988/MisakaNet)** — failure-memory MCP server for coding agents

  [![Lessons](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/Ikalus1988/MisakaNet/data/badges/lessons.json)](https://github.com/Ikalus1988/MisakaNet/tree/main/lessons)
  [![Domains](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/Ikalus1988/MisakaNet/data/badges/domains.json)](https://github.com/Ikalus1988/MisakaNet/tree/main/lessons)
  [![MCP Tools](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/Ikalus1988/MisakaNet/data/badges/tools.json)](https://github.com/Ikalus1988/MisakaNet/blob/main/scripts/mcp_server.py)
  [![Stars](https://img.shields.io/github/stars/Ikalus1988/MisakaNet?style=social)](https://github.com/Ikalus1988/MisakaNet/stargazers)

  - 🚀 **Remote MCP**: `https://misakanet.org/mcp` — 7 tools, no install, no account needed to read
  - ⚡ **One-command setup**: `npx @misaka-net/misakanet-setup` — wires **10 agents** (Claude Code / Codex / Cursor / Gemini CLI / GitHub Copilot / OpenCode / Kiro / Hermes / OpenClaw / codewhale), pre-allows the read-only tools, and prints a redacted health report
  - 🎯 **Intake Ways**: anonymous MCP intake · the intake bot as a published GitHub Action — `uses: Ikalus1988/MisakaNet@v1`, CI failure → lesson suggestion (needs `actions: read`) · registered node with a stable `client_id`
  - 🔍 **Quality machinery**: a provenance gate in CI (a cited source must resolve — placeholders and 404s fail the build), an opt-in auto-merge channel for lesson PRs, `--report --strict` for CI health gating, **three required checks** on `main` (DCO, the ubuntu test leg, and a `gate`), a weekly **mutation audit** (a gate that stays green when its check is disabled *fails* the audit), and a weekly **output audit** (an automation that runs but produces nothing fails the run)
- **[state-of-the-repo](https://github.com/Ikalus1988/MisakaNet/blob/main/docs/maintainer/state-of-the-repo.md)** — a publishable snapshot of what runs, what gates can be trusted, and what needs a human decision, so a second maintainer can pick it up without anyone's private notes
- **[fatal-guard](https://github.com/Ikalus1988/MisakaNet/tree/main/packages/fatal-guard)** — capture fatal crashes in Node.js without patching upstream. Zero deps.
- **[misakanet-core](https://github.com/Ikalus1988/misakanet-core)** — zero-dependency BM25 + RRF search engine. `pip install misakanet-core`
- **[Industrial RAG](https://github.com/Ikalus1988/self-grow-wiki)** — hybrid retrieval for FANUC robot manuals.
- 🐚 [pre-commit-dco](https://github.com/Ikalus1988/pre-commit-dco) — 43 行 Python 代码，独立验证 DCO sign-off。

## 🚀 Quick Start

**Option 1 — Remote MCP (30 seconds):**
```bash
curl -sS https://misakanet.org/mcp \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"misakanet_search","arguments":{"query":"database locked"}}}'
```

**Option 2 — Local MCP:**
```bash
git clone https://github.com/Ikalus1988/MisakaNet.git && cd MisakaNet
python3 scripts/mcp_server.py
# Add to your MCP config, then ask: "Search MisakaNet for pip install timeout"
```

**Option 3 — PyPI:**
```bash
pip install misakanet
misakanet "database is locked"
```

## 📚 MCP Tools (7 on the remote endpoint)

| Tool | Auth | Description |
|------|------|-------------|
| `misakanet_search` | **Anonymous ok** | Search failure lessons by error text / keyword (anonymous and **unlimited** — the daily read cap was removed on 2026-09-18) |
| `misakanet_get_lesson` | **Anonymous ok** | Fetch one lesson by id or path (anonymous, unlimited) |
| `misakanet_submit_intake` | **Anonymous ok** | Report a gap or ask a question → triaged GitHub issue |
| `misakanet_me_events` | **Anonymous ok** | Reuse evidence for a lesson (helpful votes, cross-node confirmations) — deliberately open, so trust can be checked before it is relied on |
| `misakanet_register` | Not required | Register a pseudonymous node → `node_id` + 30-day token, which **unlocks the write tools**; pass `client_id` to keep the same node |
| `misakanet_write_lesson` | Bearer | Submit a structured lesson (`title`/`domain`/`problem`/`root_cause`/`fix`) → lesson-gate |
| `misakanet_preflight` | Bearer | Risk check before a destructive operation |

Reads need no account, no email, and no GitHub login; a registered node is a pseudonym, not a login. Registration is not the way in — it only unlocks the write tools (`misakanet_write_lesson` / `misakanet_preflight`). The authoritative statement of what needs auth is [`AGENTS.md` §3](https://github.com/Ikalus1988/MisakaNet/blob/main/AGENTS.md) — this page mirrors it, and a mirror rots (it kept advertising the removed daily read cap for four days). The local stdio server (`python3 scripts/mcp_server.py`) exposes a few extra maintenance tools.

→ [misakanet.org](https://misakanet.org) 在线搜索所有 failure lessons，MCP 直连 Cursor / Claude / Codex。
→ [docs/integrations/mcp-remote.md](https://github.com/Ikalus1988/MisakaNet/blob/main/docs/integrations/mcp-remote.md) 完整接入指南

📝 [ikalus1988.github.io](https://ikalus1988.github.io/) — case studies, field notes, and project journeys.

🆕 **[Five Failures That Passed Every Check](https://ikalus1988.github.io/cases/five-failures-that-passed-every-check.html)** — the September log: four pull requests with 24/24 green checks citing a repository that does not exist, a tokenizer that silently dropped every Chinese query, a red check that was telling the truth, a clone 112 commits behind reality, and three security holes in the gate built to prevent exactly that.
