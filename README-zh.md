# less-is-more

> **你的判断力是方向盘。AI 是引擎。**

一套为 **Claude Code** 打造的个人 AI 协同开发 harness —— 两条安装路径，一个核心理念：**用最少的约束，达成最大的掌控力**。

---

**Other Languages**: [English](README.md)

---

## 这是什么

Claude Code 很强。但没有工作流约束，你会收到一堆不是你想要的代码。规则太多呢，你花在管理 agent 上的时间比写代码还多。

大多数 harness 的解法是**加更多**——更多规则、更多阶段、更多护栏。这个项目反其道而行：**做减法**。

6 条铁律。非琐碎任务 3 个阶段。3 个安全 hook。就这样。每条约束都必须证明自己的存在价值——不能直接保护你、不能直接放大 agent 产出，就不该在这里。

就像马鞍：它不告诉马往哪跑、跑多快，只是确保你在需要时能拉得住缰绳。马跑它最擅长的，你决定去哪。

## 为什么做这个

AI 时代，**品味是分水岭**。谁都能生成代码。差距在于理解深度：知道该保留什么、该砍掉什么、为什么。

这个 harness 把这套哲学写进 `~/.claude/`，让它对每个 session、每个项目、每个 agent 自动生效。

**核心循环：**
- **谋而后动** —— agent 先复述你的需求、等你确认、再给方案、再等一次。不会有"先干了再说"。
- **该结构化时才结构化** —— 琐碎修改？直接做。非琐碎任务？走 `/plan` → `/code` → `/evaluate`。简单事不搞繁文缛节。
- **默认安全** —— 硬编码密钥被拦截，危险命令需要你明确点头。
- **每次都在学习** —— 每次 session 结束 prompt 你记录踩坑。几周后，你就有了一份个人知识库。

## 快速开始

```bash
git clone --recursive <仓库地址>
cd less-is-more
```

然后选一条路：

| 路径 | 时间 | 一句话 |
|---|---|---|
| **一键快装** | ~15 分钟 | 粘贴一段 prompt，装完能用。个性化以后慢慢补。 |
| **深度适配** | 1-2 小时 | 写下你自己的编码哲学（"Soul"），让 agent 围绕它构建 harness。 |

**一键快装**：在本目录打开 Claude Code，粘贴 `path2-quick-install/BOOTSTRAP.md`。

**深度适配**：填好 `path1-deep-adaptation/soul-template.md`，然后粘贴 `path1-deep-adaptation/BOOTSTRAP.md`。

两条路随时切换。先用起来，再慢慢调。或者第一天就深度定制，看你自己。

## 你能得到什么

| | |
|---|---|
| **6 条协作铁律** | `CLAUDE.md` —— 谋而后动、缺信息就问、少即是多、精确优于完整 |
| **三段式工作流** | `/plan` → `/code` → `/evaluate`，每阶段专属 agent |
| **密钥扫描** | `secret-scan.sh` —— 硬编码密钥进不了你的代码库 |
| **危险命令拦截** | `dangerous-command.sh` —— `rm -rf`、`git push --force` 等触发系统弹窗，默认"拒绝" |
| **踩坑记录** | `gotchas-prompt.sh` —— session 结束自动 prompt，经验追加到个人知识库 |
| **月度复盘/审计** | `/harness-retro` + `/harness-audit` |
| **4 个专项 agent** | `planner` · `evaluator` · `code-reviewer` · `security-reviewer` |
| **4 个核心 skill** | `skill-creator` · `doc-templates` · `eval-rubric` · `gotchas` |
| **工具库** | 3 个精选 submodule —— `agent-toolkit` · `anthropic-skills` · `everything-claude-code` |

## 怎么选

```
写过自己的编码哲学 / 个人工作流规则？
  │
  ├─ 没有  →  一键快装
  │           先跑起来，边用边调。
  │
  └─ 有    →  深度适配
              agent 读你的 "Soul"，围绕它构建定制 harness。
```

拿不准？先一键快装。深度适配永远是一次对话的距离。

## 项目结构

```
less-is-more/
├─ path1-deep-adaptation/
│  ├─ BOOTSTRAP.md          ← 粘给 Claude Code
│  ├─ soul-template.md       ← 填你自己的编码哲学
│  ├─ soul-example-yijiang.md← 原作者参考范例
│  ├─ builder-guide.md
│  └─ agent-steps.md
│
├─ path2-quick-install/
│  ├─ BOOTSTRAP.md           ← 粘给 Claude Code
│  ├─ INSTALL.md             ← 给人看的安装说明
│  ├─ agent-steps.md
│  └─ claude-home/           ← 拷到 ~/.claude/ 的整包
│     ├─ CLAUDE.md
│     ├─ settings.json
│     ├─ agents/
│     ├─ commands/
│     ├─ hooks/
│     └─ skills/
│
└─ library/                  ← 开源工具（Git submodules）
   ├─ agent-toolkit/
   ├─ anthropic-skills/
   └─ everything-claude-code/
```

## 安全设计

agent **动手前必问**。没有例外。

- 改什么、改哪里，先列清楚
- 任何改动前先把 `~/.claude/` 打包备份
- 删除/覆盖必须明确授权
- `settings.json` 永远增量合并，不整份替换

如果 agent 没问就开始：打断它，说"停，先解释"。

## 扩展工具库

装完后 `~/.claude/library/` 是你的，随便加。harness 没有的能力？跟 Claude 说"去 library 里找"，它会在里面搜索匹配的 agent、skill、hook 或 command，按需拷到项目级别。

任何开源的 Claude Code 工具包（或你自己写的），直接丢到 `~/.claude/library/` 就行。

```
~/.claude/library/
├─ agent-toolkit/
├─ anthropic-skills/
├─ everything-claude-code/
└─ 某个新包/          ← 直接放这里
```

> **TODO**：未来做成 `/add-to-library` 命令，管理更方便。

## 前置要求

- **macOS**（hook 用 `osascript` 弹窗；Linux/Windows → 换成 `zenity` / PowerShell dialog）
- **Claude Code CLI** 已安装
- `~/.claude/` 目录存在（首次运行自动创建）

## 常见问题

| | |
|---|---|
| hook 不生效 | `chmod +x ~/.claude/hooks/*.sh` + 重启 Claude Code |
| 想回退 | `tar -xzf ~/.claude/backups/pre-install-*.tar.gz -C ~` |
| Agent 卡住 | 把报错贴回去，让它自己排查 |
| library/ 为空 | `git submodule update --init --recursive` |

## 参与贡献

欢迎 PR！更好的 hook、新的 skill、改进的 agent —— 直接开 issue 或提 PR。

比较大的改动建议先开 issue 对齐方向，免得你白忙活。

## 许可证

MIT。详见 [LICENSE](LICENSE)。

---

Made by [Jyan](https://github.com/jjblaack)
