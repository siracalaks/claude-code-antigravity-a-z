# 🎯 Claude Code + Google Antigravity — The A-to-Z Mastery Reference

> **🌍 Languages:** **English Guide (this file)** · [Türkçe Rehber](REHBER.md)
> [← Back to README](README.md)

**Version:** 1.0 — May 13, 2026
**Goal:** Find everything in a single file — ship without context-switching to scattered docs.
**Scope:** Producing professional projects with natural-language coding inside Antigravity IDE + Claude Code extension.

> **NOTE — MCP vs MVP confusion:**
> - **MCP** = Model Context Protocol — open protocol that lets Claude talk to external services (GitHub, databases, browsers)
> - **MVP** = Minimum Viable Product — software terminology, "the smallest shippable version of a product"
> This document covers MCP in depth and also includes a chapter on "how to ship your first MVP with Claude."

---

## 📑 Table of Contents

1. [The Big Picture — Architecture & Philosophy](#1-big-picture)
2. [Google Antigravity A-Z](#2-antigravity-a-z)
3. [Claude Code A-Z](#3-claude-code-a-z)
4. [Slash Commands — Full Reference](#4-slash-commands)
5. [Skills — The Capability System](#5-skills)
6. [MCP — Model Context Protocol](#6-mcp)
7. [Subagents — Parallel Minds](#7-subagents)
8. [Hooks — Automatic Triggers](#8-hooks)
9. [Plugins — Packaged Extensions](#9-plugins)
10. [settings.json — All Settings](#10-settings)
11. [Workflow Patterns — Professional Usage](#11-workflows)
12. [MVP Production Playbook](#12-mvp)
13. [Self-Updating Documentation Agent](#13-self-updating-agent) ⭐
14. [Emergency Cheat Sheet](#14-cheat-sheet)
15. [References](#15-references)

---

<a name="1-big-picture"></a>
## 1. 🌍 The Big Picture — Architecture & Philosophy

### How Antigravity and Claude Code Relate

Antigravity is an **agent-first IDE** (a VS Code fork). Claude Code is an **agentic CLI** that runs as a terminal tool or extension. Used together, they form a three-layer system:

```
┌─────────────────────────────────────────────────────────┐
│  ANTIGRAVITY (IDE Layer)                                │
│  - Editor View: coding, inline edits, agent chat        │
│  - Manager View: multi-agent orchestration, artifacts   │
│  - Built-in browser, terminal, Knowledge Base           │
│  - Models: Gemini 3.1 Pro/Flash, Claude, GPT-OSS        │
└──────────────────────┬──────────────────────────────────┘
                       │ as an extension
┌──────────────────────▼──────────────────────────────────┐
│  CLAUDE CODE (Agent Layer)                              │
│  - CLAUDE.md, skills, subagents, hooks, plugins         │
│  - 13 lifecycle hook events                             │
│  - MCP server connections                               │
│  - Slash commands (60+ built-in)                        │
└──────────────────────┬──────────────────────────────────┘
                       │
┌──────────────────────▼──────────────────────────────────┐
│  MCP SERVERS (External World Layer)                     │
│  GitHub, Postgres, Playwright, Context7, Linear, ...    │
└─────────────────────────────────────────────────────────┘
```

### Philosophy: Who Does What?

| Layer | Strong at | Weak at |
|---|---|---|
| **Antigravity Gemini** | Planning, architecture, large context (2M tokens), browser testing, parallel agents | Deep multi-file refactors |
| **Claude Code** | Multi-file reasoning, refactoring, debugging, terminal work | Browser integration, visual work |
| **MCP Servers** | Specific service access (DB, API) | General reasoning |
| **You (human)** | Judgment, architectural decisions, final approval | Repetitive boilerplate |

**The golden rule of professional use:** Gemini thinks (plans), Claude builds, MCP connects (data), you approve (judgment).

---

<a name="2-antigravity-a-z"></a>
## 2. 🚀 Google Antigravity A-Z

### 2.1 Two Main Views

#### Editor View
A traditional VS Code experience. Switch to **Manager View** with `Cmd+E` / `Ctrl+E`.
- **Sidebar chat** (`Cmd+L`): quick chat with context from the open file
- **Inline edit** (`Cmd+I`): direct changes to selected code
- **Tab completion**: autocomplete while typing
- **Terminal**: built-in, the agent can run commands here
- **Built-in browser**: the agent can open and test web apps

#### Manager View (Mission Control)
The real differentiator. You run **multiple agents in parallel**.
- **Workspace**: each workspace = one project folder
- **Conversation**: each chat = one agent instance
- **Worktree toggle**: creates a separate git worktree per conversation (so parallel agents don't collide)
- **Status indicator**: which agent is doing what, what's awaiting approval
- **Max 5 parallel agents** (preview limit)

### 2.2 Modes

| Mode | What it does | When |
|---|---|---|
| **Planning Mode** | Produces a plan artifact first, waits for approval | Complex feature, risky work |
| **Execution Mode** | Jumps straight to coding (vibe coding) | UI tweaks, small bugfixes |
| **Plan-Review-Execute** | Plan → comment → update → code | Default, solid approach |

### 2.3 Artifacts — Closing the Trust Gap

The agent produces an **artifact** for every job. Artifact = verifiable deliverable. Types:

1. **Task Lists** — what's to be done, which files change
2. **Implementation Plans** — architectural plan, how each function works
3. **Code Diffs** — preview of the changes
4. **Screenshots** — visual proof of UI changes
5. **Browser Recordings** — video of E2E test flows
6. **Walkthroughs** — what the agent did, step by step

**Usage rule:** **Comment** on the artifact like Google Docs; the agent reads your comments and applies fixes (without breaking flow).

### 2.4 Knowledge Base (Brain)

The `.gemini/antigravity/brain/` folder = the project's **persistent memory**. As the agent learns things (e.g., "we use Tailwind, not Bootstrap"), it writes them here. Subsequent agents read this file.

**Manual entry:** Settings → Knowledge → Add Knowledge Item. Example entries:
- "All API endpoints start with `/api/v1/`"
- "Database access only via server actions"
- "Customer X color palette: #1a1a1a, #FF6B35"

### 2.5 Agent Settings (Settings → Agent)

| Setting | Recommended | Why |
|---|---|---|
| **Artifact Review Policy** | Asks for Review | See every artifact, prevent wrong direction |
| **Terminal Command Auto Execution** | Request Review | Approval for dangerous commands like `rm -rf` |
| **Terminal Sandbox** | Enabled | Keep it inside the workspace |
| **Non-Workspace File Access** | Disabled | Protect sensitive files (.env, ssh keys) |
| **Browser Agent** | Enabled | Critical for E2E testing |

### 2.6 MCP Server Management (Antigravity Side)

One-click install via **Settings → Customizations → MCP**. Antigravity's **one-click MCP store** offers:
- Google ecosystem (Drive, Sheets, Docs, Calendar)
- GitHub, Figma, Slack, Notion
- "Open MCP Config" to add custom MCP servers

**Important:** MCPs installed in Antigravity are **not visible** to the Claude Code extension; Claude Code reads its own `~/.claude/settings.json`. You configure them separately.

### 2.7 Antigravity Shortcuts

| Shortcut | Function |
|---|---|
| `Cmd+E` / `Ctrl+E` | Switch Editor ↔ Manager |
| `Cmd+L` / `Ctrl+L` | Toggle sidebar chat |
| `Cmd+I` / `Ctrl+I` | Inline edit (selected code) |
| `Cmd+,` / `Ctrl+,` | Settings |
| `Cmd+P` / `Ctrl+P` | File search |
| `Cmd+Shift+P` / `Ctrl+Shift+P` | Command palette |

### 2.8 Antigravity Gotchas

- **Rate limit confusion**: Public preview thought to be a 5-hour quota; actually weekly. Heavy Claude/GPT users hit limits early.
- **Workspace-external file access**: Off by default; until you opt in, the agent cannot see `.env` (security).
- **Open VSX instead of VS Code Marketplace**: Some Microsoft-only extensions don't work; most do.
- **Gmail-only**: Workspace accounts not yet supported (as of May 2026).
- **Lag**: As context grows, RAM usage climbs; occasional window restarts help.

---

<a name="3-claude-code-a-z"></a>
## 3. 💻 Claude Code A-Z

### 3.1 Installation (3 Methods)

```bash
# 1. Native binary (recommended, fastest)
curl -fsSL https://claude.ai/install.sh | bash

# 2. Homebrew (macOS)
brew install --cask claude-code

# 3. NPM (deprecated — migrating away)
npm install -g @anthropic-ai/claude-code
```

Verify:
```bash
claude --version
claude --help
```

### 3.2 Authentication

```bash
claude auth login      # Login / switch account
claude auth status     # Current auth state
claude auth logout     # Remove credentials
```

**Claude Code extension inside Antigravity:**
With Extensions → Claude Code → Spark added, you have **two routes**:
1. **Anthropic API key** (from the Console, with credits): sign in with `/login` → billed by Anthropic
2. **Antigravity proxy** (uses Google's quota): use `antigravity-claude-proxy` → billed by Google (free during preview)

### 3.3 Starting a Session

```bash
cd ~/projects/my-project
claude                              # Open a new session
claude -c                           # Continue last session
claude -r <session-id>              # Resume a specific session
claude --from-pr <pr-url>           # Open a session tied to a PR
claude "fix the date bug in dates.ts"  # Single prompt, opens session
claude -p "review my changes"       # Non-interactive (print mode), for CI
```

### 3.4 Critical CLI Flags

| Flag | Function |
|---|---|
| `--model <name>` | Pick a model (sonnet, opus, haiku, opusplan) |
| `--print` / `-p` | Run one prompt and exit (for scripts) |
| `--output-format json` | Structured output (for CI/pipelines) |
| `--system-prompt-file <path>` | Read system prompt from file |
| `--append-system-prompt <text>` | Append to the default prompt |
| `--add-dir <path>` | Add additional folder to context |
| `--agents '<json>'` | Define dynamic subagents via CLI |
| `--debug` | Trace hook and MCP calls |
| `--continue` / `-c` | Resume last session |
| `--resume` / `-r <id>` | Resume a specific session |

### 3.5 @-Mentions (File References)

**Reference** instead of pasting — fewer tokens, more accurate:

```
@README.md                          # Add a file
@src/components/                    # Add a folder (recursive)
@https://example.com/docs           # Fetch a URL
@!`git diff HEAD`                   # Add shell output (dynamic)
```

### 3.6 Running Shell Commands

Run shell directly inside the session by prefixing with `!`:

```
!ls -la src/
!npm test
!git log --oneline -10
```

This tells Claude "run this command"; you see the output and can converse about it.

### 3.7 Keyboard Shortcuts (Interactive Mode)

| Shortcut | Function |
|---|---|
| `Shift+Tab` | Cycle modes: normal → auto-accept → plan |
| `Ctrl+O` | Toggle verbose transcript |
| `Cmd+Enter` (Mac) / `Ctrl+Enter` (Linux/Win) | Submit (in the extension) |
| `Ctrl+C` (twice) | Exit session |
| `Esc` | Cancel current operation |
| `↑` / `↓` | Navigate previous prompts |

### 3.8 Customizing Keybindings

Edit `~/.claude/keybindings.json`. Changes take effect immediately.

### 3.9 Model Selection (as of May 2026)

| Model | Alias | For | Not for |
|---|---|---|---|
| **Opus 4.7** | `opus` | Architectural decisions, hard multi-file refactors, serious debugging | Simple formatting, lint fixes |
| **Sonnet 4.6** | `sonnet` | Daily work, 80% of tasks | Tight token budget → use Haiku |
| **Haiku 4.5** | `haiku` | Formatting, small edits, lint errors | Multi-file reasoning |
| **opusplan** | `opusplan` | Opus in plan mode, Sonnet in build mode (token economy) | — |

```bash
/model opus           # Switch to Opus
/model sonnet         # Switch to Sonnet
/model haiku          # Switch to Haiku
/effort xhigh         # Opus 4.7 only, recommended for coding
```

**Warning:** Opus 4.7's new tokenizer can produce up to **35% more tokens** for the same text. "Opus for everything" is the most expensive junior mistake.

### 3.10 Configuration Hierarchy

Claude Code reads settings in this order (later overrides earlier):

1. `~/.claude/settings.json` — user-wide (every project)
2. `.claude/settings.json` — project-wide (committed to git, team-shared)
3. `.claude/settings.local.json` — local only (should be in .gitignore)
4. Enterprise managed settings — corporate policies

**The same applies** to skills, agents, commands:
- `~/.claude/skills/` — user
- `.claude/skills/` — project
- `~/.claude/agents/` and `.claude/agents/`
- `~/.claude/commands/` and `.claude/commands/` (legacy; prefer skills)

### 3.11 CLAUDE.md Hierarchy

Claude Code reads CLAUDE.md files in this order on every session:

1. `~/.claude/CLAUDE.md` — personal global rules
2. Starting from project root walking down: `./CLAUDE.md`, `./src/CLAUDE.md`, etc.
3. CLAUDE.md from folders added with `--add-dir` (if `CLAUDE_CODE_ADDITIONAL_DIRECTORIES_CLAUDE_MD=1` is set)

**All are loaded into context.** Keep them short.

---

<a name="4-slash-commands"></a>
## 4. ⚡ Slash Commands — Full Reference

Slash commands are the session's **control panel**. 60+ built-in. Categorized below.

### 4.1 Session & Context Management

| Command | Description | Pro Usage |
|---|---|---|
| `/init` | Create CLAUDE.md (set `CLAUDE_CODE_NEW_INIT=1` for interactive flow) | First command in every new project |
| `/clear` | Reset all context, fresh start | Reflex when the topic changes |
| `/compact [retain X]` | Summarize context, preserve key info | At 70% (before the default 95%) |
| `/branch` (alias `/fork`) | Fork the current session onto a new branch | "I want to try this problem differently" |
| `/ask` | Ephemeral side question (doesn't pollute main context) | "Just curious about X mid-flight" |
| `/continue` | Resume flow after compact | — |
| `/resume <id>` | Return to a specific session | — |

### 4.2 Memory & Info

| Command | Description |
|---|---|
| `/memory` | Manage CLAUDE.md and other memory files |
| `/release-notes` | Show last release notes |
| `/doctor` | System health check (for config issues) |

### 4.3 Model & Permission

| Command | Description |
|---|---|
| `/model [opus\|sonnet\|haiku\|opusplan]` | Switch model |
| `/effort [low\|medium\|high\|xhigh]` | Set reasoning effort (Opus 4.7) |
| `/fast` | Toggle Fast Mode (same model, latency-optimized API) |
| `/plan` | Toggle plan mode (each tool call asks for approval) |
| `/permissions` | Edit allow/deny rules |
| `/less-permission-prompts` | Analyze past calls and build an allowlist (v2.1.111+) |
| `/keybindings` | Edit keybindings |
| `/auto-mode` | Toggle auto mode (default on for Max sub, v2.1.112+) |

### 4.4 Extension Management

| Command | Description |
|---|---|
| `/mcp` | Manage MCP servers (list/add/remove/test) |
| `/agents` | Manage and test subagents |
| `/plugin` | Plugin marketplace + install |
| `/skills` | List skills |
| `/hooks` | Edit hook configuration |

### 4.5 Stats & Info

| Command | Description |
|---|---|
| `/usage` | Canonical usage dashboard (plan limit + rate limit + cost + daily session, v2.1.118+) |
| `/cost` | Cost tab (alias) |
| `/stats` | Daily usage, sessions, streaks (alias) |
| `/help` | List all commands (including your custom ones) |
| `/context` | Show current context usage |
| `/sessions` | Session history |

### 4.6 Theme & Look

| Command | Description |
|---|---|
| `/theme` | Pick theme / add custom theme (`~/.claude/themes/<name>.json`, v2.1.118+) |
| `/output-style` | Switch output stylesheet |

### 4.7 Workflow Helpers

| Command | Description |
|---|---|
| `/review` | Run code review on current changes |
| `/install-github-app` | Install GitHub Actions integration |
| `/team-onboarding` | Generate an onboarding guide for a teammate (built-in) |
| `/buddy` | 🐣 Easter egg: terminal pet (April Fools surprise, v2.1.89+) |

### 4.8 MCP Prompts (auto-added)

Every connected MCP server adds its own prompts. Format:
```
/mcp__<server-name>__<prompt-name>
```

Example (GitHub MCP):
```
/mcp__github__list_prs
/mcp__github__create_issue
/mcp__github__review_pr
```

### 4.9 Plugin Commands

Come from plugins, namespaced:
```
/frontend-design:frontend-design
/connect-apps:send-email
/claude-code-builder:create-skill
```

### 4.10 Custom Skills (legacy: custom commands)

The skills you write. `.claude/skills/<name>/SKILL.md` or `~/.claude/skills/<name>/SKILL.md`:

```
/my-skill
/conventional-commit
/api-endpoint
```

> **Important:** As of v2.1.101 (April 2026), **custom slash commands and skills are unified**. Old `.claude/commands/*.md` files still work, but the new format is `.claude/skills/<name>/SKILL.md`. If a skill and a command share a name, the **skill** wins.

### 4.11 Pro Patterns for Writing Commands

**Pattern 1: Argument templating**
```markdown
---
name: fix-issue
description: Fix a GitHub issue
---
Fix issue #$ARGUMENTS following our coding standards.
```
Usage: `/fix-issue 123` → `$ARGUMENTS` = `"123"`

**Pattern 2: Dynamic context injection**
```markdown
---
name: commit
description: Context-aware git commit
allowed-tools: Bash(git *)
---
## Context
- Status: !`git status`
- Diff: !`git diff HEAD`

Generate a conventional commit message and commit.
```

**Pattern 3: Model pinning (for critical commands)**
```markdown
---
name: security-audit
description: Full security audit
model: claude-opus-4-7
---
Audit for OWASP Top 10...
```

**Pattern 4: Tool restriction**
```markdown
---
name: readonly-explore
allowed-tools: Read, Grep, Glob
---
Read-only access. Do not write anything.
```

**Pattern 5: Namespacing**
```
/refactor/rename-pattern
/test/add-edge-cases
/db/migration-draft
```

---

<a name="5-skills"></a>
## 5. 🧠 Skills — The Capability System

### 5.1 What Is It?

**A skill = a markdown bundle teaching Claude how to do one type of task.**

- A **folder** + `SKILL.md` (required) + optional support files
- **Progressive disclosure**: only name+description is read at first (~100 tokens/skill), full content (<5K tokens) loads on demand
- **Open standard**: supported by Claude Code, Codex, Cursor, Gemini CLI, Antigravity, Windsurf

### 5.2 Skill vs Slash Command vs Subagent — Mental Model

| | Slash Command (legacy) | Skill | Subagent |
|---|---|---|---|
| **Trigger** | When you type `/cmd` | `/cmd` or Claude auto-decides | Claude auto, or via `/agents` |
| **Context** | Main session | Main session | **Separate context window** |
| **Purpose** | Quick prompt template | Reusable knowledge bundle | Isolated, specialized work |
| **Token cost** | High (fully loaded) | Low (progressive) | Medium (isolated but parallel) |

**Think of it as:** Skills = knowledge. Subagents = workers. Plugins = packaged teams.

### 5.3 SKILL.md Structure

```markdown
---
name: conventional-commit
description: Use this skill any time the user asks for a git commit. Writes messages in Conventional Commit format.
---

# Conventional Commit

When you create a git commit, follow these rules:

1. Start the subject line with one of: feat, fix, chore, docs, refactor, test, perf
2. Add a colon and a space, then a short imperative summary, no period
3. Maximum 72 characters
4. If the change is breaking, add ! before the colon

## Examples

Input: Added user authentication with JWT tokens
Output: feat(auth): implement JWT-based authentication

Input: Fixed null pointer in date parser
Output: fix(parser): handle null input in formatDate

## DON'T

- Never use vague messages like "WIP" or "tmp"
- Don't say "this commit" in the body; it's obviously a commit
- Don't capitalize the type
```

### 5.4 Frontmatter Fields

| Field | Required | Purpose |
|---|---|---|
| `name` | ✅ | Skill name (max 64 chars) and `/cmd` name |
| `description` | ✅ | Claude reads this to decide when to use the skill (max 200 chars). **The most critical field.** |
| `allowed-tools` | ❌ | Only allow these tools |
| `argument-hint` | ❌ | Hint for `/cmd [arg]` |
| `model` | ❌ | Which model to use |
| `disable-model-invocation` | ❌ | If `true`, only manual `/cmd` invocation; Claude can't auto-trigger |
| `user-invocable` | ❌ | If `false`, only Claude calls it automatically |
| `dependencies` | ❌ | Packages the skill needs |

### 5.5 Skill Locations (Scope)

```
~/.claude/skills/<name>/SKILL.md     # User-wide (every project)
.claude/skills/<name>/SKILL.md       # Project-wide (committed, team-shared)
plugins/<plugin>/skills/<name>/      # Comes from a plugin
```

### 5.6 Built-in Skills (Bundled)

Claude Code ships with 5+ bundled skills:
- **task-orchestration** — break complex work into subtasks
- **troubleshooting** — debug loops, root cause narrowing
- **monitoring** — repeated checks (while session is open)
- **anthropic-api-helper** — API-specific work
- **plan-management** — plan creation and tracking

### 5.7 Pro Skill Examples

#### Example 1: Test runner skill
```markdown
---
name: run-tests
description: Run tests matching a pattern. Use when user says "test", "run tests", or asks to verify changes.
allowed-tools: Bash(npm *), Bash(npx *), Read, Edit
argument-hint: [pattern]
---

Run tests matching: $ARGUMENTS

1. Detect framework (jest, vitest, pytest)
2. Run with given pattern, or all if empty
3. If failures: analyze, propose fix, re-run
4. Report: X passed, Y failed
```

#### Example 2: Clean imports skill
```markdown
---
name: clean-imports
description: Remove unused imports and sort the rest. Use any time the user asks to clean imports, sort imports, or tidy imports.
---

For each file the user asks to clean:

1. Remove imports not referenced in file
2. Sort remaining by: standard library, third-party, local
3. Group sections with blank line between

Do not touch side-effect imports (imports without name binding).
```

#### Example 3: PR description generator
```markdown
---
name: pr-description
description: Generate a PR description from git diff. Use when the user asks for a PR description, summary of changes, or "what changed".
---

## Diff
!`git diff main...HEAD`

## Commits
!`git log main..HEAD --oneline`

## Task
From the context above:

1. **Summary** (1-2 sentences): what this PR does
2. **Changes** (bulleted): what changed and why, per file
3. **Testing**: how to test it
4. **Breaking changes**: if any
5. **Screenshots**: remind to attach if UI changed
```

### 5.8 Skill Writing Best Practices

1. **Description, description, description** — the most critical field. Claude reads it to decide. Start with "Use this skill when...". Include trigger keywords.
2. **Stay under 500 lines.** Longer → add REFERENCE.md and link to it.
3. **Use imperative form**: "Do X", not "We do X".
4. **Add negative instructions**: "Do not do X" — prevents Claude from default behavior.
5. **Examples are critical**: show Input → Output patterns.
6. **Single responsibility**: don't write mega-skills. One skill, one job.

### 5.9 Testing Skills

Anthropic's **Skill Creator** skill writes skills via interactive Q&A:
```bash
# Install Anthropic's official skill creator
git clone https://github.com/anthropics/skills ~/.claude/skills-temp
cp -r ~/.claude/skills-temp/skills/skill-creator ~/.claude/skills/
```

Then in Claude Code:
```
/skill-creator
```

---

<a name="6-mcp"></a>
## 6. 🔌 MCP — Model Context Protocol

### 6.1 MVP ≠ MCP (Reminder)

- **MVP** (Minimum Viable Product): Software terminology. "The smallest shippable/testable product." From Eric Ries's *Lean Startup*.
- **MCP** (Model Context Protocol): An open protocol developed by Anthropic. To make Claude (or another LLM) talk to external services.

This document is about MCP. For the MVP playbook → [Chapter 12](#12-mvp).

### 6.2 What Is MCP?

**One sentence:** MCP is the universal adapter that tells Claude "speak to this API like so."

An MCP server exposes:
- **Tools** (do): "Open a PR on GitHub", "Run a Postgres query"
- **Resources** (read): files, docs, data
- **Prompts** (ready-made templates): `/mcp__github__create_pr` etc.

### 6.3 Transport Types

| Transport | For | Example |
|---|---|---|
| **stdio** | Local subprocess (npm/pip package) | `@modelcontextprotocol/server-filesystem` |
| **http** | Cloud-based service (modern, recommended) | Notion MCP, GitHub MCP |
| **sse** | Server-sent events (legacy, being replaced by http) | Older cloud servers |

### 6.4 Installation Commands

```bash
# Stdio (local package)
claude mcp add filesystem -- npx -y @modelcontextprotocol/server-filesystem /Users/me/projects

# Stdio with env vars
claude mcp add airtable \
  --env AIRTABLE_API_KEY=$AIRTABLE_KEY \
  -- npx -y airtable-mcp-server

# HTTP (recommended modern cloud)
claude mcp add --transport http notion https://mcp.notion.com/mcp

# HTTP with auth header
claude mcp add --transport http github-api https://api.github.com/mcp \
  --header "Authorization: Bearer $GH_TOKEN"

# SSE (legacy, prefer http when possible)
claude mcp add --transport sse my-service https://my-service.internal/mcp

# Via JSON config
claude mcp add-json myserver '{"command": "npx", "args": ["-y", "@pkg/mcp"]}'

# List
claude mcp list

# Detail
claude mcp get <name>

# Remove
claude mcp remove <name>

# Test (without persisting)
claude mcp run <name>

# Import from Claude Desktop config
claude mcp add-from-claude-desktop
```

### 6.5 MCP Inside settings.json

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_..."
      }
    },
    "postgres": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-postgres",
        "postgresql://readonly@localhost/mydb"
      ]
    },
    "playwright": {
      "command": "npx",
      "args": ["-y", "@executeautomation/playwright-mcp-server"]
    }
  }
}
```

### 6.6 Recommended MCP Stack (By Tier)

#### Tier 1 — Minimum (every developer)
| MCP | What it does | Command |
|---|---|---|
| **GitHub** | PRs, issues, code search | `claude mcp add github -- npx -y @modelcontextprotocol/server-github` (env: GH token) |
| **Context7** | Up-to-date docs (per library version) | `claude mcp add context7 -- npx -y @upstash/context7-mcp` |
| **Playwright** | Browser automation, E2E testing | `claude mcp add playwright -- npx -y @playwright/mcp` |

#### Tier 2 — Full-stack
Tier 1 plus:
| MCP | For |
|---|---|
| **Postgres** | Read-only DSN for DB schema + queries |
| **Supabase** | DB + auth + storage in one MCP |
| **Sentry** | Pulling production errors |
| **Linear** or **Jira** | Ticket integration |

#### Tier 3 — Power user
Tier 2 plus:
| MCP | For |
|---|---|
| **Memory** | Knowledge graph, cross-session memory |
| **Slack** | Team comms |
| **Brave Search** / **Exa** | Web search |
| **Figma** | Design-to-code |
| **Vercel** / **Cloudflare** | Deployment |

### 6.7 MCP Security Notes

- **Don't hardcode tokens** — use env vars
- **No write access to Postgres MCP** — use read-only DSN
- **Don't run untrusted MCP servers** — like SKILL.md, MCP can execute arbitrary code
- **Each MCP inflates context** — tool definitions get loaded. 20 MCPs = ~5000 token fixed tax
- **Tool Search feature** (new) — keeps non-essential tools out of context, saves up to 85%

### 6.8 MCP Gotchas

- **Don't copy Cursor config into Claude Code**: Cursor uses `"mcpServers"`, VS Code uses `"servers"`. Check the format.
- **`npx -y` always pulls latest**: pin versions for production agents (`@upstash/context7-mcp@1.4.2`)
- **Don't forget OAuth-only fallback**: headless agents can't complete redirects. Have an API-key fallback.
- **Don't install more than 6 MCPs**: causes instability; the agent can't decide which to use.

### 6.9 Antigravity MCP vs Claude Code MCP

| | Antigravity MCP | Claude Code MCP |
|---|---|---|
| Config file | Antigravity Settings UI | `~/.claude/settings.json` |
| One-click install | ✅ | ❌ (manual) |
| Visible to | Antigravity agents | Claude Code sessions |
| Same server, installed twice | ✅ | — |

**Practical rule:** Install the same MCPs in both for minimum friction.

---

<a name="7-subagents"></a>
## 7. 🤖 Subagents — Parallel Minds

### 7.1 What Is It?

**A subagent = a separate Claude embedded inside the main Claude.**

- Has its own context window (doesn't pollute the main chat)
- Has its own system prompt (specialized behavior)
- Has its own tool permissions (restricted authority)
- Returns its result as a summary to the main chat

**Benefit:** You isolate context-heavy work like codebase exploration. Saves 10K+ tokens.

**Cost:** The subagent is unaware of main context. Doesn't work for holistic reasoning.

### 7.2 Subagent Locations

```
.claude/agents/<name>.md       # Project agent
~/.claude/agents/<name>.md     # User agent
plugins/<plugin>/agents/       # Plugin agent
```

Conflict rule: project > user > plugin

### 7.3 Subagent File Format

```markdown
---
name: security-reviewer
description: Expert security code reviewer. Use PROACTIVELY after code changes to auth or data handling.
tools: Read, Grep, Glob, Bash
model: opus
permissionMode: plan
isolation: worktree
---

You are a senior security engineer specializing in OWASP Top 10.

When invoked:
1. Run `git diff HEAD` to see recent changes
2. Focus on files touching auth, data handling, input validation
3. Check for:
   - SQL injection risks
   - XSS vulnerabilities
   - Hardcoded credentials
   - Insecure deserialization
   - Missing rate limiting

Report findings as:
- **Critical**: must fix before merge
- **Warning**: should fix soon
- **Suggestion**: nice to have

For each finding: file:line, issue description, suggested fix.
```

### 7.4 All Frontmatter Fields

| Field | Description |
|---|---|
| `name` ⭐ | Agent name (required) |
| `description` ⭐ | When to use it (required) |
| `tools` | Allow-list: permitted tools |
| `disallowedTools` | Deny-list: forbidden tools |
| `model` | `opus`, `sonnet`, `haiku`, or full ID (`claude-opus-4-7`) |
| `permissionMode` | `plan`, `default`, `bypassPermissions` |
| `mcpServers` | Limit to specific MCPs |
| `hooks` | Agent-specific hooks |
| `maxTurns` | Max turns (prevents infinite loops) |
| `skills` | Skills to auto-load |
| `initialPrompt` | Starting message |
| `memory` | Memory file path |
| `effort` | Reasoning effort level |
| `background` | `true` runs async |
| `isolation` | `worktree` = separate git worktree |
| `color` | UI color |

### 7.5 Skills + Subagents = Superpower

You can auto-load skills into a subagent:

```markdown
---
name: api-developer
description: Implement API endpoints following team conventions
skills:
  - api-conventions
  - error-handling-patterns
  - validation-rules
---

Implement API endpoints following the preloaded skills' conventions.
```

This agent starts with three skill bodies in its context — you don't have to re-explain.

### 7.6 Pro Subagent Sets

#### Set 1: Code Quality
```
code-reviewer.md       # General review, OWASP-aware
type-checker.md        # TypeScript strict, no any
performance-auditor.md # Bottleneck detection
```

#### Set 2: Documentation
```
docs-writer.md         # JSDoc / Python docstring
readme-updater.md      # Keep README in sync
changelog-generator.md # Conventional commits → changelog
```

#### Set 3: Investigation
```
codebase-explorer.md   # "How does auth work?" exploration
bug-reproducer.md      # Isolate a bug
log-analyzer.md        # Scan production logs
```

### 7.7 Dynamic Subagent From CLI

```bash
claude --agents '{
  "code-reviewer": {
    "description": "Expert code reviewer",
    "prompt": "You are a senior code reviewer...",
    "tools": ["Read", "Grep", "Glob", "Bash"],
    "model": "sonnet"
  }
}'
```

This agent lives with the session; it's not saved to a file.

### 7.8 Agent Teams (Experimental)

Works if `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS` is set. A lead agent + multiple teammates coordinate via a shared task list. Overkill for most users; subagents are usually enough.

---

<a name="8-hooks"></a>
## 8. 🪝 Hooks — Automatic Triggers

### 8.1 What Is It? (vs. Skills)

| | Skill | Hook |
|---|---|---|
| Trigger | Claude decides | Lifecycle event |
| Runner | LLM | Shell script |
| Reliability | Model can forget | 100% deterministic |
| Purpose | Reasoning | Guaranteed action |

**Think of it as:** Prompts suggest, hooks **guarantee**. Format, lint, security gates belong in hooks.

### 8.2 The 13 Lifecycle Events

```
Session Lifecycle:
├── Setup (init/maintenance)
├── SessionStart (startup/resume/clear)
└── SessionEnd (exit/sigint/error)

Main Loop:
├── UserPromptSubmit
├── UserPromptExpansion (during slash command expansion)
└── Tool Execution
    ├── PreToolUse 🔒
    ├── PermissionRequest ❓
    ├── PostToolUse ✅
    └── PostToolUseFailure ❌

Subagent Lifecycle:
├── SubagentStart 🚀
└── SubagentStop 🏁

Maintenance:
├── PreCompact 📦
└── Notification 🔔

Termination:
└── Stop / StopFailure 🛑
```

### 8.3 Hook Configuration

Inside `.claude/settings.json`:

```json
{
  "hooks": {
    "<EventName>": [
      {
        "matcher": "<RegexPattern>",
        "hooks": [
          {
            "type": "command",
            "command": "<shell command>",
            "timeout": 30
          }
        ]
      }
    ]
  }
}
```

### 8.4 Exit Code Semantics

| Exit Code | Meaning |
|---|---|
| `0` | Success, proceed |
| `2` | **Block** — stop the tool call, stderr is shown to Claude as an error |
| Other | Non-blocking error, written to log |

### 8.5 Hook Types

```json
// 1. Shell command
{ "type": "command", "command": "npm run lint" }

// 2. HTTP webhook
{ "type": "http", "url": "http://localhost:8080/hook", "timeout": 30 }

// 3. Prompt (lightweight LLM call)
{ "type": "prompt", "prompt": "Evaluate if this is safe: $TOOL_INPUT" }

// 4. MCP tool
{ "type": "mcp", "tool": "myserver.evaluate" }
```

### 8.6 Environment Variables

Variables hooks can access:

| Variable | What |
|---|---|
| `$CLAUDE_PROJECT_DIR` | Project root path |
| `$CLAUDE_PLUGIN_ROOT` | Plugin folder (portable path) |
| `$CLAUDE_FILE_PATH` | Affected file path |
| `$CLAUDE_TOOL_NAME` | Tool name |
| `$CLAUDE_TOOL_INPUT` | Tool input JSON |
| `$CLAUDE_TOOL_RESULT` | (PostToolUse) tool result |
| `$CLAUDE_SESSION_ID` | Session ID |
| `$CLAUDE_ENV_FILE` | (SessionStart) write persistent env vars |
| `$CLAUDE_CODE_REMOTE` | Running in remote context |
| `$USER_PROMPT` | (UserPromptSubmit) prompt text |

### 8.7 Pro Hook Examples

#### Hook 1: Auto-format on file edit
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write|MultiEdit",
        "hooks": [
          {
            "type": "command",
            "command": "npx prettier --write \"$CLAUDE_FILE_PATH\" 2>/dev/null"
          }
        ]
      }
    ]
  }
}
```

#### Hook 2: Block dangerous commands
```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "if echo \"$CLAUDE_TOOL_INPUT\" | grep -qE '(rm -rf|sudo rm|chmod 777|:(){:|:&};:)'; then echo 'Dangerous command blocked!' >&2; exit 2; fi"
          }
        ]
      }
    ]
  }
}
```

#### Hook 3: Block secret file access
```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Read|Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "if echo \"$CLAUDE_FILE_PATH\" | grep -qE '\\.(env|pem|key)$'; then echo 'Secret file access blocked' >&2; exit 2; fi"
          }
        ]
      }
    ]
  }
}
```

#### Hook 4: SessionStart context loader
```json
{
  "hooks": {
    "SessionStart": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "echo '## Git Status'; git status --short; echo '## TODO Comments'; grep -rn 'TODO:' src/ 2>/dev/null | head -5"
          }
        ]
      }
    ]
  }
}
```

#### Hook 5: Test-on-commit enforcement
```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "if echo \"$CLAUDE_TOOL_INPUT\" | grep -q 'git commit'; then npm test --silent || (echo 'Tests must pass before commit' >&2 && exit 2); fi"
          }
        ]
      }
    ]
  }
}
```

#### Hook 6: Notification on completion
```json
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "osascript -e 'display notification \"Claude finished\" with title \"Claude Code\"'"
          }
        ]
      }
    ]
  }
}
```

### 8.8 Hook Performance Rules

- **Every hook runs synchronously**; its time is added to the tool call
- PostToolUse hooks over 500ms → session feels slow
- Many fast hooks > a few slow hooks
- Profile with `time`: `time bash my-hook.sh`

### 8.9 The `/hooks` Command — Interactive UI

```
/hooks                # Open hook management screen
```

Pick event → enter matcher → paste command → save. No more wrestling with JSON.

---

<a name="9-plugins"></a>
## 9. 📦 Plugins — Packaged Extensions

### 9.1 What Is It?

**A plugin = bundle of Skills + Subagents + Hooks + MCP servers + Commands.**

One plugin = all extension types under one name. One install, one update, for your whole team.

### 9.2 Plugin Installation

```bash
# Add a marketplace
/plugin marketplace add <github-org/repo>

# Install a plugin
/plugin install <plugin-name>@<marketplace>

# List
/plugin list

# Details
/plugin details <plugin-name>

# Uninstall
/plugin uninstall <plugin-name>
```

### 9.3 Marketplace Structure

Inside a plugin repo:
```
my-plugin-marketplace/
├── .claude-plugin/
│   └── marketplace.json     # Plugin index
└── plugins/
    └── my-plugin/
        ├── .claude-plugin/
        │   └── plugin.json  # Manifest
        ├── agents/
        ├── skills/
        ├── commands/
        ├── hooks/
        └── README.md
```

### 9.4 Recommended Plugin Repos

| Repo | Contains | For |
|---|---|---|
| `wshobson/agents` | 81 plugins, 200+ skills | General use |
| `anthropics/skills` | Official skill examples | Learning |
| `ccplugins/awesome-claude-code-plugins` | Curated plugin list | Discovery |
| `alirezarezvani/claude-skills` | 245+ skills (engineering, marketing, compliance) | Broad coverage |
| `hyperskill/claude-code-marketplace` | Hyperskill team plugins | Production-ready |
| `alexanderop/claude-code-builder` | Plugin/skill/agent builder | New plugin developers |

### 9.5 Plugin Authoring Mini-Tutorial

`.claude-plugin/plugin.json`:
```json
{
  "name": "my-awesome-plugin",
  "version": "1.0.0",
  "description": "Does awesome things",
  "author": "Your Name",
  "repository": "github.com/you/my-plugin"
}
```

`.claude-plugin/marketplace.json` (if you want your own marketplace):
```json
{
  "name": "my-marketplace",
  "plugins": [
    {
      "name": "my-awesome-plugin",
      "path": "./plugins/my-awesome-plugin"
    }
  ]
}
```

Local test:
```bash
cd my-plugin-repo
claude
/plugin marketplace add .
/plugin install my-awesome-plugin@my-marketplace
```

---

<a name="10-settings"></a>
## 10. ⚙️ settings.json — All Settings

### 10.1 Location Hierarchy (reminder)

1. `~/.claude/settings.json` — user
2. `.claude/settings.json` — project (committed)
3. `.claude/settings.local.json` — local override (.gitignore)
4. Enterprise managed settings — top priority

### 10.2 Full Schema (Complete Example)

```json
{
  "model": "claude-sonnet-4-6",
  "maxTokens": 4096,
  "autoCompactThreshold": 0.7,
  "effort": "high",
  "fastMode": false,

  "permissions": {
    "allowedTools": [
      "Read",
      "Write",
      "Edit",
      "Bash(git *)",
      "Bash(npm *)",
      "Bash(npx *)"
    ],
    "deny": [
      "Read(./.env)",
      "Read(./.env.*)",
      "Write(./production.config.*)",
      "Bash(rm -rf *)",
      "Bash(sudo *)"
    ],
    "ask": [
      "Bash(git push *)",
      "Write(./public/*)"
    ]
  },

  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write(*.py)",
        "hooks": [
          {
            "type": "command",
            "command": "python -m black \"$CLAUDE_FILE_PATH\""
          }
        ]
      },
      {
        "matcher": "Edit|Write|MultiEdit",
        "hooks": [
          {
            "type": "command",
            "command": "npx prettier --write \"$CLAUDE_FILE_PATH\" 2>/dev/null"
          }
        ]
      }
    ],
    "SessionStart": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "git status --short && echo '---' && git log --oneline -5"
          }
        ]
      }
    ]
  },

  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_..."
      }
    },
    "context7": {
      "command": "npx",
      "args": ["-y", "@upstash/context7-mcp"]
    },
    "playwright": {
      "command": "npx",
      "args": ["-y", "@playwright/mcp"]
    }
  },

  "subagents": {
    "autoDelegate": true,
    "maxParallel": 5
  },

  "skills": {
    "autoInvoke": true
  },

  "ui": {
    "theme": "tokyo-night",
    "statusLine": "tokens-and-cost"
  },

  "enabledPlugins": [
    "frontend-design",
    "connect-apps"
  ]
}
```

### 10.3 Permissions — Granular Control

Three levels:
- **`allowedTools`**: permitted tools (allow-list)
- **`deny`**: hard block (always)
- **`ask`**: ask for confirmation (interactive)

**Pattern matching:** `Bash(git *)` = any command starting with `git`. `Read(./.env)` = only the `.env` file.

### 10.4 Status Line Customization

To keep an eye on token count, a custom status line:

```bash
# ~/.claude/statusline.sh
#!/bin/bash
cat <<EOF
${CLAUDE_MODEL} | ${CLAUDE_TOKENS_USED}/${CLAUDE_TOKENS_TOTAL} tokens | \$${CLAUDE_COST}
EOF
```

```json
{
  "ui": {
    "statusLine": "/Users/me/.claude/statusline.sh"
  }
}
```

### 10.5 Environment Variables

| Var | Description |
|---|---|
| `CLAUDE_CODE_NEW_INIT=1` | Interactive `/init` flow |
| `CLAUDE_CODE_ADDITIONAL_DIRECTORIES_CLAUDE_MD=1` | Also load CLAUDE.md from `--add-dir` folders |
| `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` | Agent Teams (experimental) |
| `CLAUDE_CODE_REMOTE` | Running in remote context (signals hooks) |
| `ANTHROPIC_DEFAULT_OPUS_MODEL` | Pin Opus model for Bedrock/Vertex |

---

<a name="11-workflows"></a>
## 11. 🎯 Workflow Patterns — Professional Usage

### 11.1 Pattern A: Plan-Build-Verify Loop

**Scenario:** A new, risky/complex feature.

```
1. Antigravity Manager → open a new conversation (Gemini Pro)
2. Prompt: "Plan only, don't code: <feature description>"
3. Gemini produces an Implementation Plan artifact
4. Use Google-Docs-style comments to refine
5. Once approved → switch to Editor View
6. Open Claude Code in the terminal
7. Hand the plan to Claude: "Implement the plan in /artifacts/plan-001.md"
8. Claude implements
9. E2E test via Playwright MCP
10. Have Claude write the PR description
```

**Win:** Gemini tokens = cheap/free (Antigravity quota). Claude tokens = implementation only. 40-60% savings.

### 11.2 Pattern B: Parallel Multi-Agent (Worktree)

**Scenario:** Three independent tasks (frontend + backend + test).

```
1. Antigravity Manager View
2. Open 3 conversations, each with "Worktree toggle ON"
3. Agent 1 (frontend): "Build UserCard component, components/UserCard.tsx"
4. Agent 2 (backend): "Add /api/users endpoint with validation"
5. Agent 3 (test): "Write E2E test for user registration flow"
6. 3 agents run in parallel in Manager View
7. Each produces an artifact
8. On approval → merge each worktree to main
```

**Win:** One developer → 3x throughput.

### 11.3 Pattern C: Iterative Refinement (Comment-Driven)

**Scenario:** UI tweaks, customer feedback.

```
1. Antigravity Editor View
2. Use sidebar chat to generate a component (Gemini Flash, fast)
3. See it in the browser preview
4. Select a region → Cmd+I → "Make this card more rounded, add shadow"
5. Re-check
6. Customer says "make the color warmer" → write a comment → agent applies
```

**Win:** No flow break, fast iteration.

### 11.4 Pattern D: Bug Investigation (Subagent-Heavy)

**Scenario:** Weird production bug.

```
1. Open Claude Code
2. /agents → "Use codebase-explorer to find all auth-related files"
3. Result returns (subagent context is isolated)
4. /agents → "Use log-analyzer to scan /var/log/app.log for errors today"
5. Result returns
6. Back in main chat, merge them: "Combine findings, propose root cause"
7. Then implement
```

**Win:** Main context stays clean, 20K+ tokens saved.

### 11.5 Pattern E: Spec-Driven Development

**Scenario:** Customer brief → working product.

```
1. Capture the brief as markdown: brief.md
2. Hand to Gemini: "Convert this to PRD with user stories and acceptance criteria"
3. PRD approved → "Generate architecture diagram (mermaid)"
4. Architecture approved → "Generate task breakdown (list of issues)"
5. Each task → its own Claude Code conversation
6. Per conversation: implement + test + PR
7. End with a browser-recorded demo (Antigravity)
```

**Win:** Brief → MVP in 2-3 days.

### 11.6 Workflow Specific to Claude Code Extension Inside Antigravity

How the Claude Code extension lives inside Antigravity:

1. **Open**: The Claude Code panel opens in the sidebar
2. **Auth**: First open, sign in with `/log` (Anthropic or Antigravity proxy)
3. **MCP**: MCPs installed in Antigravity **don't automatically appear** in Claude Code. Install them separately in `~/.claude/settings.json`
4. **`/init`**: For an existing project, call `claude` in the terminal → `/init`
5. **`/mcp`**: You can also manage MCPs via Extensions → Claude Code → Spark in Antigravity
6. **Division of labor**: Keep Gemini in the sidebar, Claude Code in the terminal/panel

---

<a name="12-mvp"></a>
## 12. 🚀 MVP Production Playbook (software term)

**MVP = Minimum Viable Product.** The smallest product a customer will actually pay for. Shippable in 1-3 days with Antigravity + Claude Code.

### 12.1 The 7-Step MVP Shipment

```
Day 1 morning   → BRIEF
Day 1 noon      → PRD + ARCHITECTURE
Day 1 evening   → SCAFFOLD
Day 2 morning   → AUTH + DATABASE
Day 2 noon      → CORE FEATURES
Day 2 evening   → UI POLISH + TEST
Day 3           → DEPLOY + DEMO
```

### 12.2 Step 1: Brief → PRD (Antigravity / Gemini)

```
"You are a product manager. Convert this rough idea into a PRD:

[customer brief]

Output:
1. User personas (3 max)
2. User stories (in format: As a X, I want Y, so that Z)
3. Acceptance criteria per story (Given/When/Then)
4. Non-functional requirements
5. Out of scope (what we WON'T build)
6. Success metrics

Keep ruthless. MVP is 'minimum viable'."
```

### 12.3 Step 2: PRD → Architecture (Antigravity / Gemini)

```
"From the PRD above, propose:

1. Tech stack (justify each choice)
2. Folder structure
3. Database schema (mermaid ER diagram)
4. API endpoints (RESTful)
5. Auth strategy
6. Deployment target
7. Estimated cost ($)

Constraints:
- Stack: Next.js + Supabase + Vercel (unless strong reason not to)
- Time budget: 3 days
- Must be deployable to production"
```

### 12.4 Step 3: Scaffold (Claude Code)

```bash
cd ~/projects
mkdir my-mvp && cd my-mvp
claude
```

```
"Following the architecture in /Users/me/Documents/architecture.md, scaffold a Next.js 15 project:

1. Initialize with create-next-app (TypeScript, Tailwind, App Router)
2. Set up Supabase client
3. Create folder structure per architecture
4. Add CLAUDE.md with stack info and conventions
5. Initialize git, first commit

After scaffold, run npm run dev and confirm it works."
```

### 12.5 Step 4: Auth + DB (Claude Code, Supabase MCP)

```
"Implement authentication:

1. Supabase Auth with email/password
2. Login, signup, logout pages
3. Protected route middleware
4. User profile page
5. Migration for users table extension (if needed)

Use Supabase MCP to:
- Create the migration
- Apply it
- Test the auth flow

Test with Playwright MCP: signup → email confirm (mock) → login → profile → logout."
```

### 12.6 Step 5: Core Features

A separate Claude conversation per feature:

```
"Feature: <name from PRD>

Acceptance criteria:
<paste from PRD>

Constraints:
- Match existing component patterns (see /components)
- Use server actions, not API routes
- Add Vitest tests for business logic
- Add Playwright test for happy path

When done:
- Show me the diff
- Show me how to test manually
- Suggest commit message"
```

### 12.7 Step 6: UI Polish (Antigravity inline)

In Antigravity Editor View, select the component → `Cmd+I`:
- "Make this more polished with subtle animations"
- "Add loading states"
- "Improve accessibility (ARIA labels, keyboard nav)"
- "Make mobile-responsive"

### 12.8 Step 7: Deploy (Claude Code + Vercel MCP)

```
"Deploy to Vercel:

1. Check there are no console.log()s left
2. Check no hardcoded secrets
3. Set environment variables in Vercel (use Vercel MCP)
4. Configure custom domain if provided
5. Deploy preview first
6. Run smoke tests against preview URL (Playwright MCP)
7. If green, promote to production
8. Create release notes from git log

Output: production URL + smoke test results."
```

### 12.9 MVP Anti-Patterns

❌ **Doing all features in one conversation** → context bloats, quality drops
✅ Each feature in its own conversation, parallel via worktree

❌ **Leaving tests till the end** → cheap during, expensive after
✅ Each feature: implement + test in the same conversation

❌ **Skipping auth/security because "we'll add it later"**
✅ Auth is one of the first steps; you can't bolt it onto a finished product

❌ **Stuffing 10 features into the MVP** → that's a V1, not an MVP
✅ Max 3 user stories, the rest is V2

---

<a name="13-self-updating-agent"></a>
## 13. ⭐ Self-Updating Documentation Agent

**What you want:** an agent that scrapes the web daily for new Claude Code/Antigravity developments and updates this doc. Reasonable — clever, even. Here's how to build it.

### 13.1 Architecture

```
┌──────────────────────────────────────────────────────────┐
│  Cron job (daily 09:00)                                  │
│    │                                                      │
│    ▼                                                      │
│  Call Claude Code CLI (-p print mode, JSON output)       │
│    │                                                      │
│    ▼                                                      │
│  Skill: doc-updater + Subagent: research-scout           │
│    │                                                      │
│    ├─→ Use WebFetch to scan recent blog posts            │
│    ├─→ Pull recent release notes via GitHub MCP          │
│    ├─→ Read current doc, find deltas                     │
│    └─→ Add new sections, update old ones                 │
│         (also write to CHANGELOG.md)                      │
│    │                                                      │
│    ▼                                                      │
│  Git commit + push (or open a PR automatically)          │
│    │                                                      │
│    ▼                                                      │
│  Notification (Slack/Discord/email)                      │
└──────────────────────────────────────────────────────────┘
```

### 13.2 Implementation Steps

#### Step 1: Create the subagent — `~/.claude/agents/research-scout.md`

```markdown
---
name: research-scout
description: Daily reconnaissance for new Claude Code and Antigravity features, releases, and best practices. Use ONLY when invoked by the doc-updater skill.
tools: WebFetch, WebSearch, Read, Grep
model: sonnet
maxTurns: 20
isolation: worktree
---

You are a research scout for Claude Code and Google Antigravity ecosystem updates.

Your mission, when invoked:

1. Check these sources for updates in the last 7 days:
   - https://code.claude.com/docs/en/release-notes
   - https://developers.googleblog.com (Antigravity tag)
   - https://github.com/anthropics/claude-code/releases
   - https://github.com/hesreallyhim/awesome-claude-code (recent commits)
   - https://github.com/anthropics/skills (recent commits)

2. For each new piece of info, classify it as:
   - **NEW FEATURE** (new slash command, hook event, etc.)
   - **DEPRECATION** (something removed/changed)
   - **BEST PRACTICE** (community pattern)
   - **TOOL** (new MCP server, skill, plugin)

3. Return a structured report:

```yaml
date: YYYY-MM-DD
items:
  - type: NEW_FEATURE
    title: "..."
    description: "..."
    source_url: "..."
    affected_section: "..."  # Which section of this doc needs updating?
    suggested_change: "..."
```

4. Do NOT modify any files. You only research.
5. If nothing significant found, return: `{ date: "...", items: [] }`
```

#### Step 2: Create the skill — `~/.claude/skills/doc-updater/SKILL.md`

```markdown
---
name: doc-updater
description: Update the Claude Code + Antigravity reference document with new findings. Use when invoked by the daily cron or when user says "update docs".
allowed-tools: Read, Edit, Write, Bash(git *)
---

# Documentation Updater

When invoked:

1. **Invoke the research-scout subagent** with: "Run daily reconnaissance"

2. Wait for structured report.

3. If `items` is empty: log "No updates today" and exit.

4. For each item in the report:
   - Read the affected section of `~/Documents/claude-code-antigravity-reference.md`
   - Make a minimal, surgical edit (don't rewrite, just add/update)
   - Add an entry to `CHANGELOG.md` at the top:
     ```
     ## YYYY-MM-DD
     - [type] Brief description (source)
     ```

5. After all updates:
   - Commit changes: `git add -A && git commit -m "docs: daily auto-update $(date +%Y-%m-%d)"`
   - If on a tracked branch, push: `git push`

6. Output a summary:
   - Number of changes
   - Sections affected
   - Link to commit

## RULES

- Do NOT rewrite working sections
- Do NOT remove content unless source explicitly deprecates it
- DO preserve the document's structure (table of contents, anchors)
- DO add "Updated: YYYY-MM-DD" to changed sections
- If unsure about a change, create an entry in `pending-review.md` instead of editing
```

#### Step 3: Daily trigger via hooks — `~/.claude/settings.json`

Note: Claude Code hooks are tied to user sessions; cron needs an **external scheduler**.

#### Step 4: Cron script — `~/scripts/daily-doc-update.sh`

```bash
#!/bin/bash
set -e

LOG_FILE=~/logs/claude-doc-update-$(date +%Y%m%d).log
mkdir -p ~/logs

cd ~/Documents/claude-docs

# Run headless
claude -p "/doc-updater run daily update" \
  --output-format json \
  --model sonnet \
  > "$LOG_FILE" 2>&1

# Error check
if [ $? -ne 0 ]; then
  echo "Doc update failed, see $LOG_FILE"
  # Send notification
  osascript -e 'display notification "Doc update failed" with title "Claude Auto-Doc"'
  exit 1
fi

# Success: extract summary
SUMMARY=$(jq -r '.result' "$LOG_FILE")
echo "$SUMMARY"

# macOS notification
osascript -e "display notification \"Updated docs. See $LOG_FILE\" with title \"Claude Auto-Doc\""
```

Cron entry (on macOS, prefer `launchd`; Linux example):
```bash
crontab -e
# Run daily at 09:00
0 9 * * * /Users/me/scripts/daily-doc-update.sh
```

#### Step 5: macOS launchd (modern alternative to cron)

`~/Library/LaunchAgents/com.me.claudedocupdate.plist`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>Label</key>
  <string>com.me.claudedocupdate</string>

  <key>ProgramArguments</key>
  <array>
    <string>/Users/me/scripts/daily-doc-update.sh</string>
  </array>

  <key>StartCalendarInterval</key>
  <dict>
    <key>Hour</key>
    <integer>9</integer>
    <key>Minute</key>
    <integer>0</integer>
  </dict>

  <key>StandardOutPath</key>
  <string>/Users/me/logs/launchd-doc-update.log</string>

  <key>StandardErrorPath</key>
  <string>/Users/me/logs/launchd-doc-update.err</string>
</dict>
</plist>
```

Load:
```bash
launchctl load ~/Library/LaunchAgents/com.me.claudedocupdate.plist
launchctl start com.me.claudedocupdate
```

### 13.3 Improvements

**V2 ideas:**

1. **Add a Web Search MCP** (Brave or Exa) — broader sweep than Claude's built-in WebSearch
2. **Notification webhook** — write changes to Slack/Discord
3. **PR mode** — open a PR instead of committing directly (for review)
4. **Embedding-based dedup** — don't add the same news twice
5. **Source priority score** — changes from anthropics/ org = GOLD; random blog = SILVER
6. **Weekly digest** — Sunday morning summary instead of daily noise

### 13.4 Self-Updating Doc Agent Alternative: GitHub Actions

Cleaner if you already use CI/CD:

`.github/workflows/doc-update.yml`:

```yaml
name: Daily Doc Update
on:
  schedule:
    - cron: '0 9 * * *'  # UTC
  workflow_dispatch:

jobs:
  update:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Install Claude Code
        run: curl -fsSL https://claude.ai/install.sh | bash

      - name: Run doc updater
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          claude -p "/doc-updater run daily update" \
            --output-format json \
            --model sonnet

      - name: Create PR if changes
        uses: peter-evans/create-pull-request@v6
        with:
          commit-message: "docs: daily auto-update"
          title: "Auto-update docs $(date +%Y-%m-%d)"
          branch: auto-doc-update-$(date +%Y%m%d)
```

Runs in the cloud, keeps history in GitHub.

---

<a name="14-cheat-sheet"></a>
## 14. 🆘 Emergency Cheat Sheet

```
═══════════════════════════════════════════════════════════
TOKEN EMERGENCY
═══════════════════════════════════════════════════════════
Tokens running out         /compact (retain X)
Topic changed              /clear
Side question              /ask
Try alternative path       /branch (was: /fork)
Check cost                 /cost
Plan vs actual             /usage

═══════════════════════════════════════════════════════════
MODEL QUICK SWITCH
═══════════════════════════════════════════════════════════
Light work                 /model haiku
Daily work                 /model sonnet
Heavy work                 /model opus
Opus for plan, Sonnet for build   /model opusplan
High effort (Opus 4.7)     /effort xhigh

═══════════════════════════════════════════════════════════
EXTENSION MANAGEMENT
═══════════════════════════════════════════════════════════
Manage MCP                 /mcp
Manage subagents           /agents
List skills                /skills
Install plugin             /plugin install <name>@<market>
Edit hooks                 /hooks

═══════════════════════════════════════════════════════════
FILE REFERENCES
═══════════════════════════════════════════════════════════
Add a file                 @path/to/file
Add a folder               @path/to/dir/
Fetch a URL                @https://...
Command output             !`git diff HEAD`
Inline shell               !ls -la

═══════════════════════════════════════════════════════════
KEYBOARD
═══════════════════════════════════════════════════════════
Mode cycle                 Shift+Tab
Submit (extension)         Cmd/Ctrl+Enter
Cancel                     Esc
Exit                       Ctrl+C, Ctrl+C
Editor↔Manager (Antigrav)  Cmd/Ctrl+E
Inline edit (Antigrav)     Cmd/Ctrl+I

═══════════════════════════════════════════════════════════
PROMPT FORMULA
═══════════════════════════════════════════════════════════
   Context     (which file, which function)
+  Reproducer  (input → expected → actual)
+  Constraint  (what NOT to do)
+  Done        (definition of done)
+  Output      (diff or full file?)
═══════════════════════════════════════════════════════════

THE 7 RULES OF TOKEN ECONOMY:
1. Keep CLAUDE.md short (<1200 tokens)
2. /compact threshold at 70% (not default 95%)
3. Right model (Sonnet default, Opus exception)
4. Don't paste long logs → read from file
5. /clear reflex (whenever topic changes)
6. Subagent for exploration (context isolation)
7. Don't send "thank you" messages

ANTIGRAVITY + CLAUDE CODE ROLE SPLIT:
- Gemini (Antigravity sidebar) → planning, architecture, quick questions
- Claude Code (terminal/panel)  → implementation, refactor, debug
- MCP servers                   → DB, GitHub, Playwright, etc.
- You                           → judgment, architectural decisions, approval
```

---

<a name="15-references"></a>
## 15. 📚 References (Read In Order)

### Official Documentation
- https://code.claude.com/docs — Main Claude Code docs
- https://code.claude.com/docs/en/commands — Slash commands
- https://code.claude.com/docs/en/skills — Skills
- https://code.claude.com/docs/en/sub-agents — Subagents
- https://code.claude.com/docs/en/hooks — Hooks
- https://platform.claude.com/docs — Claude API & Agent SDK
- https://developers.googleblog.com/build-with-google-antigravity/ — Antigravity intro
- https://codelabs.developers.google.com/getting-started-google-antigravity — Antigravity codelab

### GitHub Repos
- https://github.com/anthropics/claude-code — Official Claude Code repo
- https://github.com/anthropics/skills — Official skill examples
- https://github.com/hesreallyhim/awesome-claude-code — Main awesome list
- https://github.com/jqueryscript/awesome-claude-code — Tool integrations
- https://github.com/ccplugins/awesome-claude-code-plugins — Plugin list
- https://github.com/ComposioHQ/awesome-claude-skills — 1000+ skills
- https://github.com/wshobson/agents — 81-plugin marketplace
- https://github.com/alirezarezvani/claude-skills — 245+ cross-platform skills
- https://github.com/luongnv89/claude-howto — Visual guide

### Marketplaces & Directories
- https://buildwithclaude.com/ — Official plugin marketplace
- https://awesomeclaude.ai/ — Visual directory
- https://mcp-marketplace.io/ — MCP server marketplace
- https://sub-agents.directory — Subagent catalog

### Community
- r/ClaudeAI (Reddit)
- r/ChatGPTCoding (Reddit, cross-community)
- Anthropic Discord
- Claude Code GitHub issues

### Anthropic Official Training
- https://anthropic.skilljar.com — Anthropic Academy
- https://anthropic.skilljar.com/introduction-to-model-context-protocol — MCP course

---

## 📝 Closing

This document was assembled on May 13, 2026. The ecosystem updates weekly — especially:
- Claude Code versions (v2.1.x)
- New slash commands
- New hook events
- Skill standard expansions

**Set up the Self-Updating Agent from Chapter 13 today**, and within a week this doc stays current on its own. Skip the manual research — as promised.

If you have questions, paste this doc to Claude and chat about it. The document is a living artifact, not a frozen book.

**Safe travels.** 🚀
