# less-is-more

> **Your judgment is the steering wheel. AI is the engine.**

A personal AI development harness for **Claude Code** — two installation paths, one philosophy: **the minimum number of constraints that still keep you in control**.

---

**Other Languages**: [中文](README-zh.md)

---

## What Is This

Claude Code is powerful. But without a workflow, you end up reviewing a mountain of code you didn't ask for. With *too many* rules, you spend more time managing the agent than actually shipping.

Most harnesses solve this by adding **more** — more rules, more phases, more guardrails. This one solves it by adding **less**.

Six iron rules. Three phases for non-trivial work. Three safety hooks. That's it. Every constraint earns its place by either protecting you directly or multiplying the agent's output. Everything else is noise.

Think of it like reins on a horse: they don't dictate the route or the pace. They just make sure you can steer when it matters.

## Why It Exists

In the AI era, **taste is the differentiator**. Anyone can generate code. The gap is between people who understand what to keep, what to cut, and why.

This harness bakes that philosophy into `~/.claude/` so it applies to every session, every project, every agent — automatically.

**The core loop:**
- **Think first** — the agent restates your ask, waits for your "yes", proposes a plan, waits again. No drive-by commits.
- **Structure when it matters** — trivial? Go direct. Non-trivial? `/plan` → `/code` → `/evaluate`. No ceremony for simple stuff.
- **Safety on by default** — secrets blocked, destructive commands need your explicit go-ahead.
- **Learn from every session** — session end prompts you to capture what went wrong. Weeks later, you've built a personal knowledge base.

## Quick Start

```bash
git clone --recursive <repo-url>
cd less-is-more
```

Then pick your path:

| Path | Time | TL;DR |
|---|---|---|
| **Quick Install** | ~15 min | Paste one prompt, get a working harness. Personalize later. |
| **Deep Adaptation** | 1-2 hrs | Write your own coding philosophy ("Soul"), let the agent build around it. |

**Quick Install**: Open Claude Code in this directory, paste `path2-quick-install/BOOTSTRAP.md`.

**Deep Adaptation**: Fill out `path1-deep-adaptation/soul-template.md`, then paste `path1-deep-adaptation/BOOTSTRAP.md`.

Paths are interchangeable. Start quick, refine later. Or go deep from day one.

## What You Get

| | |
|---|---|
| **6 Iron Rules** | `CLAUDE.md` — think-first, ask-when-unclear, less-is-more, precision-over-completeness |
| **3-Phase Workflow** | `/plan` → `/code` → `/evaluate` with dedicated agents |
| **Secret Scanner** | `secret-scan.sh` — blocks hardcoded keys before they hit your repo |
| **Dangerous Command Guard** | `dangerous-command.sh` — `rm -rf`, `git push --force` etc. trigger a system dialog (defaults to "deny") |
| **Gotcha Capture** | `gotchas-prompt.sh` — end-of-session prompt, auto-appends to your personal knowledge base |
| **Monthly Retro & Audit** | `/harness-retro` + `/harness-audit` |
| **4 Specialist Agents** | `planner` · `evaluator` · `code-reviewer` · `security-reviewer` |
| **4 Core Skills** | `skill-creator` · `doc-templates` · `eval-rubric` · `gotchas` |
| **Tool Library** | 3 curated submodules — `agent-toolkit` · `anthropic-skills` · `everything-claude-code` |

## How to Choose

```
Written down your own coding philosophy / workflow rules?
  │
  ├─ No  →  Quick Install
  │          Get it running. Tweak as you go.
  │
  └─ Yes →  Deep Adaptation
             The agent reads your "Soul" and builds a custom harness.
```

Unsure? Start Quick Install. Deep Adaptation is always one conversation away.

## Project Structure

```
less-is-more/
├─ path1-deep-adaptation/
│  ├─ BOOTSTRAP.md          ← paste this into Claude Code
│  ├─ soul-template.md       ← fill this out with your own philosophy
│  ├─ soul-example-yijiang.md← author's reference example
│  ├─ builder-guide.md
│  └─ agent-steps.md
│
├─ path2-quick-install/
│  ├─ BOOTSTRAP.md           ← paste this into Claude Code
│  ├─ INSTALL.md             ← human-readable instructions
│  ├─ agent-steps.md
│  └─ claude-home/           ← gets copied to ~/.claude/
│     ├─ CLAUDE.md
│     ├─ settings.json
│     ├─ agents/
│     ├─ commands/
│     ├─ hooks/
│     └─ skills/
│
└─ library/                  ← open-source tools (Git submodules)
   ├─ agent-toolkit/
   ├─ anthropic-skills/
   └─ everything-claude-code/
```

## Safety by Design

The agent **asks before it acts**. Always.

- Lists exactly what it'll touch before touching anything
- Full `~/.claude/` snapshot before any changes
- Deletions/overwrites require explicit authorization
- `settings.json` is always incrementally merged, never replaced

If the agent starts acting without asking: interrupt it. Say "stop, explain."

## Extending the Library

After install, `~/.claude/library/` is yours to grow. Need a capability the harness doesn't have? Tell Claude "go look in library." It'll search for matching agents, skills, hooks, or commands and copy what's needed.

Drop any open-source Claude Code tool pack (or your own) directly into `~/.claude/library/`.

```
~/.claude/library/
├─ agent-toolkit/
├─ anthropic-skills/
├─ everything-claude-code/
└─ some-new-package/    ← just drop it here
```

> **TODO**: Will become a `/add-to-library` command for easier management.

## Prerequisites

- **macOS** (hooks use `osascript` dialogs; Linux/Windows → swap to `zenity` / PowerShell)
- **Claude Code CLI** installed
- `~/.claude/` exists (created on first run)

## Troubleshooting

| | |
|---|---|
| Hooks not firing | `chmod +x ~/.claude/hooks/*.sh` + restart Claude Code |
| Want to undo | `tar -xzf ~/.claude/backups/pre-install-*.tar.gz -C ~` |
| Agent stuck | Paste the error back, let it debug itself |
| Library/ empty | `git submodule update --init --recursive` |

## Contributing

PRs welcome. Better hook, new skill, improved agent — open an issue or drop a PR.

Big changes? Open an issue first. Let's align before you invest time.

## License

MIT. See [LICENSE](LICENSE) for details.

---

Made by [Jyan](https://github.com/jjblaack)
