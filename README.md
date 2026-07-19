# Test Automation Toolkit

MCP-driven test automation stack integrating **Playwright**, **Filesystem**, and **GitHub** servers for end-to-end browser QA orchestration via opencode CLI.

## Architecture

```
┌─────────────────────────────────────────────────┐
│                  opencode CLI                     │
├─────────────────────────────────────────────────┤
│  Playwright MCP    Filesystem MCP   GitHub MCP   │
│  (browser UI)      (artifacts)      (issues/PR)  │
└─────────────────────────────────────────────────┘
```

## MCP Servers

| Server | Tool | Role |
|---|---|---|
| **Playwright** | `@playwright/mcp` | Browser navigation, clicks, assertions, screenshots |
| **Filesystem** | `@modelcontextprotocol/server-filesystem` | Save test reports, logs, screenshots locally |
| **GitHub** | `@modelcontextprotocol/server-github` | Create issues, PRs from test results |

## Configuration

The MCPs are registered in `~/.config/opencode/opencode.json`:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "playwright": {
      "type": "local",
      "command": ["npx", "@playwright/mcp"],
      "enabled": true
    },
    "filesystem": {
      "type": "local",
      "command": ["npx", "-y", "@modelcontextprotocol/server-filesystem", "~/test-automation"],
      "enabled": true
    },
    "github": {
      "type": "local",
      "command": ["npx", "-y", "@modelcontextprotocol/server-github"],
      "enabled": true,
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

### Requirements

- **Node.js** >= 18
- **GitHub Token** set as a user environment variable `GITHUB_TOKEN` with `repo` scope
- **opencode** CLI or VS Code extension

## Usage

### Start a browser session

```powershell
npx @playwright/mcp
```

### Run a test flow

Use opencode to orchestrate: launch Playwright for browser interaction, save screenshots via Filesystem, and file bugs via GitHub.

### Save artifacts

The Filesystem server is rooted at `~/test-automation/`. All test reports, screenshots, and logs are stored here.

## Token Security

The GitHub token is stored as a **Windows User environment variable** (`GITHUB_TOKEN`), not in any configuration file. To set it:

```powershell
[Environment]::SetEnvironmentVariable("GITHUB_TOKEN", "ghp_...", "User")
```

Restart your terminal for the change to take effect.

## Project Structure

```
~/test-automation/
├── README.md           ← this file
└── reports/            ← test results & screenshots (generated)
```
