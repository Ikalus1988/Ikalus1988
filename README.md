# Hi there 👋

> 一个 Agent 踩过的坑，不该让其他 Agent 再踩一次。
>
> One agent's failure shouldn't be another agent's lesson — that's why MisakaNet exists.

- **[MisakaNet](https://github.com/Ikalus1988/MisakaNet)** — failure-memory MCP server for coding agents. 244 indexed lessons, 18 domains. ⭐ 327 stars
- **[fatal-guard](https://github.com/Ikalus1988/MisakaNet/tree/main/packages/fatal-guard)** — capture fatal crashes in Node.js without patching upstream. Zero deps.
- **[misakanet-core](https://github.com/Ikalus1988/misakanet-core)** — zero-dependency BM25 + RRF search engine. `pip install misakanet-core`
- **[Industrial RAG](https://github.com/Ikalus1988/self-grow-wiki)** — hybrid retrieval for FANUC robot manuals. 190+ PDFs, 230k+ chunks.
- 🐚 [pre-commit-dco](https://github.com/Ikalus1988/pre-commit-dco) — 43 行 Python 代码，拒绝 pygrep，独立验证 DCO sign-off。

**MisakaNet MCP 一行接入：**

```bash
pip install misakanet-core
```

```json
{ "mcpServers": { "misakanet": { "command": "python", "args": ["-m", "misakanet_core.server"] } }
```

→ [misakanet.org](https://misakanet.org) 在线搜索所有 failure lessons，MCP 直连 Cursor / Claude / Codex。

📝 [ikalus1988.github.io](https://ikalus1988.github.io/) — case studies, field notes, and project journeys.
