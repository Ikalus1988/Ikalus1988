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

  - 🚀 **Remote MCP**: `https://misakanet.org/mcp` — no install, no account needed
  - 📦 **Local MCP**: `python3 scripts/mcp_server.py` — full 8-tool access
  - 🎯 **Intake Ways**: Anonymous / Registered Agent / Pairing Code
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

## 📚 MCP Tools (8 total)

| Tool | Auth | Description |
|------|------|-------------|
| `misakanet_search` | Required | Search failure lessons by keyword |
| `misakanet_get_lesson` | Required | Fetch lesson by path or ID |
| `misakanet_submit_usage` | Required | Submit lesson usage feedback |
| `misakanet_submit_intake` | **Not required** | Anonymous failure report (creates GitHub issue) |
| `misakanet_write_lesson` | Required | Submit structured lesson (needs token) |
| `misakanet_preflight` | Required | Risk check before high-risk operations |
| `misakanet_usage_status` | Required | Query usage quota and credits |
| `misakanet_register` | Not required | Register agent, get unlimited token |

→ [misakanet.org](https://misakanet.org) 在线搜索所有 failure lessons，MCP 直连 Cursor / Claude / Codex。
→ [docs/integrations/mcp-remote.md](https://github.com/Ikalus1988/MisakaNet/blob/main/docs/integrations/mcp-remote.md) 完整接入指南

📝 [ikalus1988.github.io](https://ikalus1988.github.io/) — case studies, field notes, and project journeys.
