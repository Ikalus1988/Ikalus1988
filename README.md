# Hi there 👋

> 一个 Agent 踩过的坑，不该让其他 Agent 再踩一次。
>
> One agent's failure shouldn't be another agent's lesson — that's why MisakaNet exists.

## 🔧 What I Build

- **[MisakaNet](https://github.com/Ikalus1988/MisakaNet)** — failure-memory MCP server for coding agents · **v2.50.1**

  [![Lessons](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/Ikalus1988/MisakaNet/data/badges/lessons.json)](https://github.com/Ikalus1988/MisakaNet/tree/main/lessons)
  [![Domains](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/Ikalus1988/MisakaNet/data/badges/domains.json)](https://github.com/Ikalus1988/MisakaNet/tree/main/lessons)
  [![MCP Tools](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/Ikalus1988/MisakaNet/data/badges/tools.json)](https://github.com/Ikalus1988/MisakaNet/blob/main/scripts/mcp_server.py)
  [![Stars](https://img.shields.io/github/stars/Ikalus1988/MisakaNet?style=social)](https://github.com/Ikalus1988/MisakaNet/stargazers)

  - 🚀 **Remote MCP**: `https://misakanet.org/mcp` — **8 tools**, no install, no account needed to read
  - ⚡ **One-command setup**: `npx @misaka-net/misakanet-setup` — wires **10 agents**, pre-allows the read-only tools, and prints a redacted health report. Full walkthrough below, because "one command" hides three separate things it installs.
  - 🧩 **DeepSeek Harness plugin**: `dsh plugin --profile web add misakanet` — the same library as a native host surface: a `/misakanet <query>` command that answers in the composer, a right-pane panel (what this session searched, what came back, what was filed, the reuse counters), a General-settings row, and a frame-wide toast — in **zh/en**
  - 🛰️ **Discovery beyond MCP**: `llms.txt` / `llms-full.txt`, an A2A agent card at `.well-known/agent-card.json`, and a WebMCP surface for browsers
  - 🎯 **Intake Ways**: anonymous MCP intake · the intake bot as a published GitHub Action — `uses: Ikalus1988/MisakaNet@v1`, CI failure → lesson suggestion (needs `actions: read`) · registered node with a stable `client_id`
  - 🔍 **Quality machinery**: a provenance gate in CI (a cited source must resolve — placeholders and 404s fail the build), an opt-in auto-merge channel for lesson PRs, `--report --strict` for CI health gating, **four required checks** on `main` (DCO, the ubuntu test leg, `gate`, and `audit`), a **behavioural e2e** that drives the DSH client half in a real host and a real Chromium (the source-level gates can only see *registrations*, never whether anything happens), a weekly **mutation audit** (a gate that stays green when its check is disabled *fails* the audit), and a weekly **output audit** (an automation that runs but produces nothing fails the run)
  - 📦 **Open dataset** — the corpus and every benchmark query set at [data.misakanet.org](https://data.misakanet.org/latest.json): versioned (`v<date>-<sha>`), per-file SHA-256, corpus Apache-2.0 and benchmark artifacts CC0, so a retrieval number can be re-run without cloning anything or asking anyone
  - 📄 **[state-of-the-repo](https://github.com/Ikalus1988/MisakaNet/blob/main/docs/maintainer/state-of-the-repo.md)** — a publishable snapshot of what runs, what gates can be trusted, and what needs a human decision, so a second maintainer can pick it up without anyone's private notes
- **[fatal-guard](https://github.com/Ikalus1988/MisakaNet/tree/main/packages/fatal-guard)** — capture fatal crashes in Node.js without patching upstream. Zero deps.
- **[misakanet-core](https://github.com/Ikalus1988/misakanet-core)** — zero-dependency BM25 + RRF search engine. `pip install misakanet-core`
- **[pensieve](https://github.com/Ikalus1988/pensieve)** — procedural memory for agents: validated routines as YAML cards over an SQLite FTS5 index, injected as a *path + anchor* instead of a wall of text. Deliberately not a general memory layer.
- **[工业 RAG 知识库](https://ikalus1988.github.io/rag-journey/)** — hybrid retrieval + SAG search over 190+ FANUC robot manuals, 230k+ chunks. The build journal is public; the knowledge base itself is a private repo, so this link is the demo, not a download.
- 🐚 **[pre-commit-dco](https://github.com/Ikalus1988/pre-commit-dco)** — 43 行 Python 代码，独立验证 DCO sign-off。The standalone sibling `check-dco` and the archaeology in `dco-audit` live in their own repos; the [write-up of the upstream fight](https://ikalus1988.github.io/cases/check-dco-pre-commit-battle.html) is public.

## 🚀 Quick Start

**Option 1 — one command, ten agents (recommended):**
```bash
npx @misaka-net/misakanet-setup
# close and reopen the assistant window, then ask something that would normally
# invite a guess: "switch vision model" · "tool call permission denied"
```

**Option 2 — Remote MCP (30 seconds, no install):**
```bash
curl -sS https://misakanet.org/mcp \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"misakanet_search","arguments":{"query":"database locked"}}}'
```

**Option 3 — Local MCP:**
```bash
git clone https://github.com/Ikalus1988/MisakaNet.git && cd MisakaNet
python3 scripts/mcp_server.py
# Add to your MCP config, then ask: "Search MisakaNet for pip install timeout"
```

**Option 4 — PyPI:**
```bash
pip install misakanet
misakanet "database is locked"
```

**Option 5 — the library in your own code:**
```bash
pip install misakanet-core   # BM25 + RRF, stdlib only, CJK-aware tokenization
```

> Same name, different things: `pip install misakanet` is the **CLI/stdio server**, `pip install misakanet-core` is the **search library**, and the npm package `misakanet` is the **DSH plugin bundle**. Two of those three carry the DSH-era version number (2.50.1); the library tracks on its own (2.7.0).

## 🧩 The setup installer, in full

`@misaka-net/misakanet-setup` (v0.7.1, MIT, **Node 18+, zero dependencies**) is the package
the one-liner above runs. It is worth a paragraph, because a tool that "sets up an agent"
can be doing three unrelated jobs, and only two of them are the same job.

### What one command actually installs

| # | Goal | Mechanism | Who gets it |
|---|---|---|---|
| 1 | the agent **can** call it | the remote endpoint `https://misakanet.org/mcp` written into the host's own config | all 10 |
| 2 | the agent **knows when** | a rules block — imperative wording ("必须先调 misakanet_search"), because a field test showed a weaker model reads a descriptive block as background and never calls the tool | Claude Code · Codex · Hermes · OpenClaw · codewhale |
| 3 | the checkpoint **fires** | a turn-counting hook: turn 1 announces the install, turn 20 and every 10 after injects a "distil and submit" reminder, and a failed tool call injects a "search before you retry" built from the error text | **Claude Code only** |
| 4 | *(opt-in)* you **hear** it | `--voice` adds a `PostToolUse` hook playing the cue the server asked for | Claude Code, opt-in |

Without (2) a rule saying "search first" is decorative — agents don't keep counters, so without
(3) the checkpoint never arrives either; and (3) has nothing to call without (1).

### Which file each of the ten agents gets

| Agent | File(s) it writes |
|---|---|
| Claude Code | `~/.claude.json` · `~/.claude/settings.json` · `~/.claude/CLAUDE.md` |
| Codex | `~/.codex/config.toml` · `~/.codex/AGENTS.md` |
| Hermes | `~/.hermes/config.yaml` · `~/.hermes/SOUL.md` |
| OpenClaw | `mcp.servers.misakanet` in `~/.openclaw/openclaw.json` · workspace `AGENTS.md` |
| codewhale | `~/.codewhale/mcp.json` · a project `AGENTS.md` when a project is named |
| Cursor | `~/.cursor/mcp.json` → `mcpServers.url` |
| Gemini CLI | `~/.gemini/settings.json` → `mcpServers.**httpUrl**` (its `url` means SSE) |
| Copilot CLI | `~/.copilot/mcp-config.json` → `mcpServers.url`, `type: "http"` |
| OpenCode | `~/.config/opencode/opencode.json` → **`mcp`**.url, `type: "remote"` |
| Kiro | `~/.kiro/settings/mcp.json` → `mcpServers.url` |

Those last five are the honest edge of the tool: their rules live in **project-scoped** files
(`.cursor/rules/*.mdc`, `GEMINI.md`, `.github/copilot-instructions.md`, `AGENTS.md`, `.kiro/steering/*.md`)
and an installer cannot know where your projects live — so it writes the MCP entry, then tells you
the project-side action per client instead of implying behaviour changed. Each file also uses its
own key path and URL field; **a copied entry with the wrong key fails silently** — no error, the server
just never appears.

### Flags worth knowing

```bash
--dry-run      # print what would change, write nothing (also lists what it *cannot* change)
--verify       # READY / NOT READY, endpoint probe, hook, MCP registration, installed vs latest
--report       # this machine's state as redacted YAML — safe to paste into an issue or a CI log
--report --strict   # (= --ci) the same report, but the exit code IS the verdict: 0 READY / 1 NOT READY / 2 could not run
--report-json  # same document as JSON (`misakanet-setup-report/1`), stdout only, for MDM/GPO
--silent       # enterprise: no progress narration; every `!` line, report and error still prints
--only claude,codex   # one agent only
--mcp-only     # register the endpoint and change nothing else — for permission-tiered fleets
--no-register  # read-only install: no anonymous node, no token
--uninstall    # remove exactly what it added
```

`--report` alone always exits 0 on purpose — a report is evidence meant to be collected, and a
nonzero exit turns "collect the evidence" into a failing step. The gate is the opt-in `--strict`
form. The convention is the repo-wide one (`0 = fine, 1 = found problems, 2 = could not run`), so a
CI step can tell "the machine is unhealthy" from "the checker broke".

### Safety, and how to undo it

- Every rewritten file is backed up to `<file>.misakanet.bak` first; everything it adds sits
  between `misakanet:start` / `misakanet:end` markers, so `--uninstall` removes exactly its own
  work and leaves your hooks, MCP servers and TOML keys alone.
- Idempotent: a second run changes nothing. Updating is one command — nothing is ever replaced
  behind your back, and the upgrade nudge is silent for the first 14 days.
- The token lives at `~/.misakanet-agent/token` (mode 600), goes into your local config only, and
  is never printed — no subprocess is spawned, so it never reaches a command line. `--verify` probes
  the endpoint anonymously.
- Lessons retrieved from the server are **data, not instructions** — the injected rules tell the
  assistant not to execute commands found in them.
- Enterprise boundary questions (what stays on the machine, what must reach `misakanet.org`, how
  `--no-register` avoids registration entirely): [`docs/maintainer/enterprise-deployment.md`](https://github.com/Ikalus1988/MisakaNet/blob/main/docs/maintainer/enterprise-deployment.md) and [misakanet.org/enterprise](https://misakanet.org/enterprise/) — which also lists what does **not** exist yet (no compliance certifications, no org-level audit export, no self-hosted endpoint).
- Behind a corporate proxy: `NODE_USE_ENV_PROXY=1` (Node ≥ 22.21) — otherwise the probe can fail
  while your agent reaches the endpoint fine, and the report says so (`endpoint-probe: inconclusive-proxy`) instead of lying.
- WSL/Linux users have a Python installer too: `integrations/agent-autostart/install_misakanet_agent.py`. Both write the same markers, so either can verify or undo the other.

📖 [package README](https://github.com/Ikalus1988/MisakaNet/blob/main/packages/misakanet-setup/README.md) ·
[CI job you can copy](https://github.com/Ikalus1988/MisakaNet/blob/main/docs/maintainer/setup-health-ci.md) ·
[voice hooks](https://github.com/Ikalus1988/MisakaNet/blob/main/docs/integrations/mcp-voice-hooks.md)

## 📚 MCP Tools (8 on the remote endpoint)

| Tool | Auth | Description |
|------|------|-------------|
| `misakanet_search` | **Anonymous ok** | Search failure lessons by error text / keyword (anonymous and **unlimited** — the daily read cap was removed on 2026-09-18) |
| `misakanet_get_lesson` | **Anonymous ok** | Fetch one lesson by id or path (anonymous, unlimited) |
| `misakanet_submit_intake` | **Anonymous ok** | Report a gap or ask a question → triaged GitHub issue |
| `misakanet_me_events` | **Anonymous ok** | Reuse evidence for a lesson (helpful votes, cross-node confirmations) — deliberately open, so trust can be checked before it is relied on |
| `misakanet_me_inbox` | **Anonymous ok** | Check a previously submitted intake (`answered` / `pending` / `converted` / `not_found`) without re-submitting — rate-limited |
| `misakanet_register` | Not required for reads | Register a pseudonymous node → `node_id` + 30-day token, which **unlocks the write tools**; pass `client_id` to keep the same node |
| `misakanet_write_lesson` | Bearer | Submit a structured lesson (`title`/`domain`/`problem`/`root_cause`/`fix`) → lesson-gate |
| `misakanet_preflight` | Bearer | Risk check before a destructive operation |

Reads need no account, no email, and no GitHub login; a registered node is a pseudonym, not a login.
Registration is not the way in — it only unlocks the write tools (`misakanet_write_lesson` /
`misakanet_preflight`), which is why the two Bearer rows are the only ones marked so. The
authoritative statement of what needs auth is [`AGENTS.md` §3](https://github.com/Ikalus1988/MisakaNet/blob/main/AGENTS.md) — this page
mirrors it, and a mirror rots: this one kept advertising the removed daily read cap for four days,
and kept saying "7 tools" for as long as the endpoint had been serving 8. The local stdio server
(`python3 scripts/mcp_server.py`) exposes a few extra maintenance tools.

→ [misakanet.org](https://misakanet.org) 在线搜索所有 failure lessons，MCP 直连 Cursor / Claude / Codex。
→ [docs/integrations/mcp-remote.md](https://github.com/Ikalus1988/MisakaNet/blob/main/docs/integrations/mcp-remote.md) 完整接入指南

**Where it is listed**: [Glama](https://glama.ai/mcp/servers/Ikalus1988/MisakaNet) (tool definitions rated **A**) · [dsh-plugin.org](https://dsh-plugin.org/plugins/ikalus1988/misakanet) · [DSH Directory](https://dsh.directory/plugins/ikalus1988/misakanet) · [dsh.so](https://www.dsh.so/artifact/misakanet/) · [MCP Toplist](https://mcptoplist.com/server/io.github.Ikalus1988%2Fmisakanet) · [Smithery](https://smithery.ai/servers/misakanet/misakanet) · [HOL Registry](https://hol.org/registry/plugins/Ikalus1988%2FMisakaNet)

📝 [ikalus1988.github.io](https://ikalus1988.github.io/) — case studies, field notes, and project journeys.

🆕 **[Five Failures That Passed Every Check](https://ikalus1988.github.io/cases/five-failures-that-passed-every-check.html)** — the September log: four pull requests with 24/24 green checks citing a repository that does not exist, a tokenizer that silently dropped every Chinese query, a red check that was telling the truth, a clone 112 commits behind reality, and three security holes in the gate built to prevent exactly that.

📚 **「自动化里那些骗过我的东西」** — a 30-article Chinese series on criteria that lie, gates that never ran, and inputs nobody validated, sorted into ten failure layers (L0–L9). The workflow that produces it is a private repo; the articles are not:
[post12 · 我的浏览器自动化脚本连着骗了我七次](https://juejin.cn/post/7691872111025848320) ·
[post15 · 假成功有四种形状](https://juejin.cn/post/7692084224499204131) ·
[post27 · 62.7 秒：从「请手动删 pod」到宣布 Resolved](https://juejin.cn/post/7693439584974913536)
