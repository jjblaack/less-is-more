# harness

A personal AI development harness for **Claude Code**. Ship two installation paths — a one-click quick install, or a deep adaptation that binds to your own coding philosophy.

---

**Other Languages**: [中文](README-zh.md)

---

## Why This Exists

Claude Code is a powerful coding agent, but using it well requires **discipline** — not just better prompts. Without structure, AI-generated code accumulates fast and you lose control of what your system actually does.

This harness gives you a **repeatable workflow** that lives in `~/.claude/` and applies to every session, every project, every agent. It enforces:

- **Think before acting** — the agent must restate understanding, wait for your confirmation, then propose a plan, then wait again before touching code
- **Structured workflow** — trivial changes go direct; non-trivial work goes through `/plan` → `/code` → `/evaluate`
- **Safety by default** — hardcoded secrets get blocked, destructive commands require your explicit approval
- **Learning from mistakes** — every session end prompts you to record what went wrong, building a personal knowledge base over time

**The goal**: AI as a reliable partner, not an uncontrolled code generator.

## Quick Start

```bash
# Clone with submodules (includes the open-source tool library)
git clone --recursive <repo-url>
cd harness

# Or if already cloned but library/ is empty:
git submodule update --init --recursive
```

Then pick a path:

| Path | Time | Description |
|---|---|---|
| **One-Click Install** | ~15 min | Get a working harness immediately. Personalize later. |
| **Deep Adaptation** | 1-2 hrs | Build a harness that's tightly bound to your personal coding philosophy ("Soul"). |

**Path 1 — One-Click**: Open Claude Code in this directory, paste the contents of `path2-quick-install/BOOTSTRAP.md`, and follow the prompts.

**Path 2 — Deep Adaptation**: Fill out `path1-deep-adaptation/soul-template.md` with your own coding philosophy, then paste `path1-deep-adaptation/BOOTSTRAP.md` into Claude Code.

You can switch between paths at any time.

## Features

| Feature | How It Works |
|---|---|
| **Collaboration Iron Rules** | 6 rules in global `CLAUDE.md` — think-first, ask-when-unclear, less-is-more, precision-over-completeness |
| **Three-Phase Workflow** | `/plan` → `/code` → `/evaluate` with dedicated agents for each phase |
| **Secret Scanning** | `secret-scan.sh` hook — blocks hardcoded API keys, tokens, passwords before they land in your codebase |
| **Dangerous Command Guard** | `dangerous-command.sh` hook — `rm -rf`, `git push --force`, etc. trigger an OS dialog defaulting to "deny" |
| **Session-End Gotcha Capture** | `gotchas-prompt.sh` hook — prompts you to record lessons learned; auto-appends to your personal knowledge base |
| **Monthly Retro & Audit** | `/harness-retro` and `/harness-audit` commands to review harness gaps and scan for security issues |
| **4 Specialist Agents** | `planner`, `evaluator`, `code-reviewer`, `security-reviewer` — each with focused scope and tool access |
| **4 Core Skills** | `skill-creator`, `doc-templates`, `eval-rubric`, `gotchas` — reusable capabilities for your workflow |
| **Curated Tool Library** | 3 open-source submodules (`agent-toolkit`, `anthropic-skills`, `everything-claude-code`) — install once, expand anytime |

## Which Path Should I Pick?

```
Do you have a written coding philosophy / personal workflow rules?
  │
  ├─ No  →  Path 2: One-Click Install (15 min)
  │          Get working immediately, personalize later
  │
  └─ Yes →  Path 1: Deep Adaptation (1-2 hrs)
             The agent reads your "Soul" and builds a custom harness around it
```

**Not sure?** Start with Path 2. You can always run Path 1 later to refine it.

## Project Structure

```
harness/
├─ README.md                          ← You are here
├─ README-zh.md                       ← Chinese version
├─ LICENSE
│
├─ path1-deep-adaptation/             ← Deep adaptation path
│  ├─ BOOTSTRAP.md                    ← Entry prompt to paste into Claude Code
│  ├─ soul-template.md                ← Empty Soul template with fill-in prompts
│  ├─ soul-example-yijiang.md         ← Author's Soul (reference example)
│  ├─ builder-guide.md                ← Concise build guide (for the agent)
│  └─ agent-steps.md                  ← Step-by-step with mandatory checkpoints
│
├─ path2-quick-install/               ← One-click install path
│  ├─ BOOTSTRAP.md                    ← Entry prompt to paste into Claude Code
│  ├─ INSTALL.md                      ← Human-readable install instructions
│  ├─ agent-steps.md                  ← Agent install steps with checkpoints
│  └─ claude-home/                    ← Bundle to copy into ~/.claude/
│     ├─ CLAUDE.md                    ← Global contract (name placeholder)
│     ├─ settings.json                ← Hooks + env config (incremental merge)
│     ├─ agents/                      ← planner / evaluator / code-reviewer / security-reviewer
│     ├─ commands/                    ← /plan /code /evaluate /harness-retro /harness-audit
│     ├─ hooks/                       ← secret-scan / dangerous-command / gotchas-prompt
│     └─ skills/                      ← skill-creator / doc-templates / eval-rubric / gotchas
│
└─ library/                           ← Open-source tool collection (Git submodules)
   ├─ agent-toolkit/                  → github.com/softaworks/agent-toolkit
   ├─ anthropic-skills/               → github.com/anthropics/skills
   └─ everything-claude-code/         → github.com/affaan-m/everything-claude-code
```

## Safety Guarantees

Every path follows the same discipline — **the agent asks before it acts**:

- Lists exactly what files it will touch before modifying anything
- Takes a full `~/.claude/` snapshot backup before any changes
- Requires explicit authorization for deletions or overwrites
- Always incrementally merges `settings.json`, never overwrites your existing config

If the agent starts acting without asking: interrupt it. Say "stop, explain first."

## Expanding Your Tool Library

After installation, `~/.claude/library/` is your personal open-source tool collection. You can extend it anytime.

**How to use**: When you need a capability the harness doesn't have, tell Claude "go look in library." It searches `~/.claude/library/` for matching agents, skills, hooks, or commands, then copies what's needed to the project level.

**Adding new packages**: Drop any open-source Claude Code tool pack (or your own) directly into `~/.claude/library/`. One subdirectory per package:

```
~/.claude/library/
├─ agent-toolkit/
├─ anthropic-skills/
├─ everything-claude-code/
└─ some-new-package/    ← just drop it here
```

> **TODO**: This will become a command (e.g. `/add-to-library`) for easier management.

## Prerequisites

- **macOS** — hooks use `osascript` for native dialogs. Linux/Windows users need to adapt hooks to use `zenity` or PowerShell dialogs.
- **Claude Code CLI** installed and working
- `~/.claude/` directory exists (Claude Code creates it on first run)

## Troubleshooting

| Issue | Fix |
|---|---|
| Hooks don't trigger | `chmod +x ~/.claude/hooks/*.sh` and restart Claude Code |
| Want to uninstall | Restore from `~/.claude/backups/pre-install-*.tar.gz` |
| Agent gets stuck | Paste the error back to it and let it debug |
| Library/ is empty after clone | `git submodule update --init --recursive`

## Contributing

PRs welcome! If you have a better hook, a new skill, or an improved agent definition, open an issue or submit a PR.

For larger changes, please open an issue first so we can discuss the approach.

## License

MIT. See [LICENSE](LICENSE) for details.
