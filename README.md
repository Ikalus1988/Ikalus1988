# Ikalus1988

I build auditable knowledge systems for AI agents.

## Things I build

| Project | What it does | Status |
|---|---|---|
| **[MisakaNet](https://github.com/Ikalus1988/MisakaNet)** | Failure-memory MCP server for coding agents. 244 indexed lessons across 18 domains. | ![PyPI](https://img.shields.io/pypi/v/misakanet-core) ![MCP](https://img.shields.io/badge/MCP-Registry-blue) |
| **[fatal-guard](https://github.com/Ikalus1988/MisakaNet/tree/main/packages/fatal-guard)** | Non-invasive crash capture for Node.js. Zero dependencies. | ![npm](https://img.shields.io/npm/v/@misaka-net/fatal-guard) |
| **[Industrial RAG](https://github.com/Ikalus1988/self-grow-wiki)** | Hybrid retrieval for FANUC robot manuals. 190+ PDFs, 230k+ chunks. | Private archive |

## Quick start

**MCP server (Claude Desktop / Cursor / Windsurf):**

```json
{
  "mcpServers": {
    "misakanet": {
      "command": "python",
      "args": ["-m", "misakanet_core.server"]
    }
  }
}
```

```bash
pip install misakanet-core
```

**Search lessons from CLI:**

```bash
python -m misakanet_core.search "DCO sign-off failed"
```

## Live stats

<!-- These update via GitHub Actions or manual refresh -->

- 📖 **244** indexed failure lessons
- 🌐 **235+** registered agent nodes
- 🏷️ **18** knowledge domains
- 📦 Published: [PyPI](https://pypi.org/project/misakanet-core/) · [npm](https://www.npmjs.com/package/@misaka-net/fatal-guard) · [MCP Registry](https://mcp.so/server/misakanet) · [Glama](https://glama.ai/mcp/servers/misakanet)

## Find me

- 🌐 [ikalus1988.github.io](https://ikalus1988.github.io/)
- 📦 [misakanet.org](https://misakanet.org)
