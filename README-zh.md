# harness

一套为 **Claude Code** 打造的个人 AI 协同开发 harness。两条路径随你挑：一键快装，或深度适配你的个人编码哲学。

---

**其他语言**: [English](README.md)

---

## 为什么做这个

Claude Code 是个强大的编程 agent，但用得好靠的是**纪律**，不是更好的 prompt。没有结构约束时，AI 生成的代码积累得快，你就失去了对系统实际在做什么的控制。

这个 harness 提供一套**可复用的工作流**，放在 `~/.claude/` 里，对每个 session、每个项目、每个 agent 都生效。它强制：

- **谋而后动** —— agent 必须先复述理解、等你确认、再提方案、再等一次确认，才能碰代码
- **结构化工作流** —— 琐碎修改直接做；非琐碎任务走 `/plan` → `/code` → `/evaluate` 三段
- **默认安全** —— 硬编码密钥被拦截，危险命令需要你的明确授权
- **从错误中学习** —— 每次 session 结束 prompt 你记录踩坑，随时间积累成个人知识库

**目标**：让 AI 成为可靠的伙伴，而不是失控的代码生成器。

## 快速开始

```bash
# 带 submodules clone（包含开源工具库）
git clone --recursive <仓库地址>
cd harness

# 如果已经 clone 但 library/ 为空：
git submodule update --init --recursive
```

然后选一条路径：

| 路径 | 时间 | 说明 |
|---|---|---|
| **一键快装** | ~15 分钟 | 立刻能用，个性化以后慢慢补 |
| **深度适配** | 1-2 小时 | 和你的个人编码哲学（"Soul"）深度绑定 |

**路径一 · 一键快装**：在分发包根目录打开 Claude Code，粘贴 `path2-quick-install/BOOTSTRAP.md` 的内容，跟着引导走。

**路径二 · 深度适配**：填好 `path1-deep-adaptation/soul-template.md`（你的个人编码哲学），然后粘贴 `path1-deep-adaptation/BOOTSTRAP.md` 给 Claude Code。

两条路径可以随时切换。

## 核心能力

| 能力 | 实现方式 |
|---|---|
| **6 条协作铁律** | 全局 `CLAUDE.md`：谋而后动、缺信息就问、少即是多、精确优于完整 |
| **三段式工作流** | `/plan` → `/code` → `/evaluate`，每阶段有专属 agent |
| **硬编码密钥拦截** | `secret-scan.sh` hook —— 在密钥进入代码库前就挡住 |
| **危险命令弹窗确认** | `dangerous-command.sh` hook —— `rm -rf`、`git push --force` 等触发系统弹窗，默认"拒绝" |
| **会话结束记踩坑** | `gotchas-prompt.sh` hook —— 每次 session 结束 prompt 你记录经验，自动追加到个人知识库 |
| **月度复盘/审计** | `/harness-retro` 和 `/harness-audit` 命令，检查 harness 缺口和安全扫描 |
| **4 个专项 agent** | `planner`、`evaluator`、`code-reviewer`、`security-reviewer`，各司其职 |
| **4 个核心 skill** | `skill-creator`、`doc-templates`、`eval-rubric`、`gotchas`，可复用的工作流能力 |
| **精选工具库** | 3 个开源 submodule（`agent-toolkit`、`anthropic-skills`、`everything-claude-code`），一次安装，随时扩展 |

## 我该选哪条路？

```
你写过自己的编码哲学 / 个人工作流规则吗？
  │
  ├─ 没有  →  路径一：一键快装（15 分钟）
  │           先用起来，个性化以后再说
  │
  └─ 有    →  路径二：深度适配（1-2 小时）
              agent 读你的 "Soul"，围绕它构建定制 harness
```

**不确定？** 先走一键快装。随时可以回头做深度适配。

## 项目结构

```
harness/
├─ README.md                          ← 英文版（你正在看的是中文版）
├─ README-zh.md                       ← 中文版
├─ LICENSE
│
├─ path1-deep-adaptation/             ← 深度适配路径
│  ├─ BOOTSTRAP.md                    ← 粘给 Claude Code 的入口 prompt
│  ├─ soul-template.md                ← 空 Soul 模板，带占位符和填写提示
│  ├─ soul-example-yijiang.md         ← 原作者 Soul 完整版（参考范例）
│  ├─ builder-guide.md                ← 精简版搭建指南（面向 agent）
│  └─ agent-steps.md                  ← agent 执行步骤 + 强制检查点
│
├─ path2-quick-install/               ← 一键快装路径
│  ├─ BOOTSTRAP.md                    ← 粘给 Claude Code 的入口 prompt
│  ├─ INSTALL.md                      ← 给人看的安装说明
│  ├─ agent-steps.md                  ← agent 安装步骤 + 强制检查点
│  └─ claude-home/                    ← 待拷到 ~/.claude/ 的整包
│     ├─ CLAUDE.md                    ← 全局协作契约（名字占位符）
│     ├─ settings.json                ← Hooks + env 配置（增量合并）
│     ├─ agents/                      ← planner / evaluator / code-reviewer / security-reviewer
│     ├─ commands/                    ← /plan /code /evaluate /harness-retro /harness-audit
│     ├─ hooks/                       ← secret-scan / dangerous-command / gotchas-prompt
│     └─ skills/                      ← skill-creator / doc-templates / eval-rubric / gotchas
│
└─ library/                           ← 开源工具集合（Git submodules）
   ├─ agent-toolkit/                  → github.com/softaworks/agent-toolkit
   ├─ anthropic-skills/               → github.com/anthropics/skills
   └─ everything-claude-code/         → github.com/affaan-m/everything-claude-code
```

## 安全保证

无论走哪条路，**agent 在动手前都会反复向你确认**：

- 修改任何文件前，先列出具体动什么
- 改动前先备份 `~/.claude/` 整包快照
- 涉及删除/覆盖必须明确授权
- `settings.json` 永远增量合并，不整份覆盖

如果 agent 没问就开始动：打断它，说"停，先解释"。

## 扩展你的工具库（Library）

安装完成后，`~/.claude/library/` 就是你个人的开源工具库，随时可以加新包。

**使用方式**：当你需要某个能力但现有 harness 没有时，告诉 Claude "去 library 里找"，它会在 `~/.claude/library/` 中搜索匹配的 agent、skill、hook 或 command。找到后，按需拷贝到项目级别使用。

**添加新工具包**：把任何开源的 Claude Code 工具包（或自己写的）直接丢到 `~/.claude/library/` 下即可。每个包一个子目录：

```
~/.claude/library/
├─ agent-toolkit/
├─ anthropic-skills/
├─ everything-claude-code/
└─ 某个新包/          ← 直接放这里
```

> **TODO**：未来会做成一个命令（如 `/add-to-library`），方便添加和管理工具包。

## 前置要求

- **macOS** —— hook 里的 `osascript` 弹窗是 macOS 原生能力，Linux/Windows 需要改 hook 换成 zenity / PowerShell dialog
- **Claude Code CLI** 已安装并能正常使用
- `~/.claude/` 目录存在（Claude Code 首次运行会自动建）

## 常见问题

| 问题 | 解决 |
|---|---|
| 装完 hook 不弹窗 | `chmod +x ~/.claude/hooks/*.sh` 并重启 Claude Code |
| 想卸载 | 从 `~/.claude/backups/pre-install-*.tar.gz` 解压还原 |
| Agent 卡壳 | 把报错原样贴回去让它自己分析 |
| library/ 为空 | `git submodule update --init --recursive` |

## 参与贡献

欢迎提交 PR！如果你有更好的 hook、新的 skill、或改进过的 agent 定义，直接开 issue 或提 PR。

比较大的改动建议先开 issue 讨论方向，再动手。

## 许可证

MIT。详见 [LICENSE](LICENSE)。
