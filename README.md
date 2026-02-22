# n8n-builder-playground

A ready-to-use environment that lets AI agents (Claude Code or any MCP-compatible assistant) build and deploy [n8n](https://n8n.io/) workflows directly to your n8n server — just describe the workflow you want.

---

## 🚀 Quick Start

### Prerequisites

- [Node.js](https://nodejs.org/) (v18 or later) — required to run the n8n-mcp server via `npx`
- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) or another MCP-compatible AI client
- A running n8n instance (self-hosted or cloud) with API access enabled

### 1. Clone the repository

```bash
git clone https://github.com/aaalfredji/n8n-builder-playground.git
cd n8n-builder-playground
```

### 2. Configure your n8n credentials

Copy the example MCP configuration and fill in your n8n server details:

```bash
cp .mcp.json.example .mcp.json
```

Then open `.mcp.json` and replace the placeholder values:

```json
{
  "mcpServers": {
    "n8n-mcp": {
      "command": "npx",
      "args": ["n8n-mcp"],
      "env": {
        "MCP_MODE": "stdio",
        "LOG_LEVEL": "error",
        "DISABLE_CONSOLE_OUTPUT": "true",
        "N8N_API_URL": "https://your-n8n-instance.example.com",
        "N8N_API_KEY": "your-n8n-api-key-here"
      }
    }
  }
}
```

**How to get your n8n API key:**
1. Open your n8n instance in a browser
2. Go to **Settings → API**
3. Click **Create an API Key**
4. Copy the key and paste it into `.mcp.json`

> ⚠️ **Security note:** `.mcp.json` is in `.gitignore` — your real credentials will never be committed to git. Do not remove it from `.gitignore`.

### 3. Install n8n-skills (recommended)

Open Claude Code in the project directory and run:

```
/plugin install czlonkowski/n8n-skills
```

This installs 7 expert skills that teach Claude how to build production-ready n8n workflows correctly (expression syntax, validation, node configuration, etc.).

### 4. Open Claude Code and start building

```bash
claude
```

Claude will automatically load the MCP configuration and the `CLAUDE.md` instructions. Just describe the workflow you want:

```
Create a workflow that receives a webhook and sends me a Slack message with the payload
```

```
Build a scheduled workflow that fetches data from the GitHub API every hour and stores it in a Google Sheet
```

```
Make an AI agent workflow that answers customer questions using GPT-4
```

Claude will use the **n8n-mcp** MCP server to look up node documentation, build the workflow JSON, validate it, and deploy it directly to your n8n server.

---

## 🏗️ How It Works

```
You (Claude Code prompt)
        │
        ▼
   CLAUDE.md          ← guides Claude's behavior for n8n workflow building
        │
        ▼
  .mcp.json           ← connects Claude to the n8n-mcp MCP server
        │
        ▼
  n8n-mcp (npx)       ← MCP server with full n8n node documentation + workflow management tools
        │
        ▼
  Your n8n server     ← workflows are created/deployed here
```

**Components:**
- **`.mcp.json`** — MCP server configuration; tells Claude Code to run the `n8n-mcp` server with your n8n credentials
- **`CLAUDE.md`** — Project instructions for Claude; guides it to check credentials, install skills, and follow best practices when building workflows
- **[n8n-mcp](https://github.com/czlonkowski/n8n-mcp)** — MCP server providing access to 1,000+ n8n node docs, templates, and workflow management tools
- **[n8n-skills](https://github.com/czlonkowski/n8n-skills)** — 7 Claude Code skills for expert-level n8n workflow building

---

## 🔧 Configuration Reference

| Variable | Description | Example |
|---|---|---|
| `N8N_API_URL` | URL of your n8n instance | `https://n8n.example.com` |
| `N8N_API_KEY` | n8n API key for authentication | `n8n_api_...` |

**Local n8n instance?** Use `http://localhost:5678` as the URL. If n8n is running in Docker on the same machine, use `http://host.docker.internal:5678` instead.

---

## 📁 Repository Structure

```
n8n-builder-playground/
├── .mcp.json.example  ← MCP server config template (copy to .mcp.json and fill in your credentials)
├── .mcp.json          ← Your local credentials (gitignored — never committed)
├── CLAUDE.md          ← AI agent instructions (auto-loaded by Claude Code)
└── README.md          ← This file
```

---

## 🔗 Related Projects

- [n8n-mcp](https://github.com/czlonkowski/n8n-mcp) — MCP server for n8n
- [n8n-skills](https://github.com/czlonkowski/n8n-skills) — Expert Claude Code skills for n8n
- [n8n](https://n8n.io/) — Workflow automation platform