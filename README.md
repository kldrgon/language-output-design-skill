# Language Output Design

一套面向 AI 助手的语言逻辑规则与渐进式 Skill，用于改善跳跃表达、机械对比、列表滥用、解释顺序失衡和语体不匹配等问题。

仓库同时提供 Cursor、OpenAI Codex 和 Claude Code 的适配文件。

## Rule 与 Skill 的关系

Rule 保存每次交流都应遵守的通用原则，包括信息入口、主线、表达形式、受众适配、限制前置和输出检查。

Skill 保存教学解释、技术文档、诊断、决策建议、总结和改写等任务需要的详细方法、场景指导和正反例。Agent 先读取 `SKILL.md`，再沿索引按需读取参考文件，避免一次加载全部内容。

Skill 只细化 Rule，不覆盖或削弱 Rule。当前工具不支持 Skill 时，仍可单独使用 Rule。

## 仓库结构

```text
.
├── README.md
├── LICENSE
├── rules/
│   └── language-logic-rule.md
├── skills/
│   └── language-output-design/
│       ├── SKILL.md
│       └── references/
└── adapters/
    ├── cursor/
    │   └── language-logic.mdc
    ├── codex/
    │   └── AGENTS.md
    └── claude-code/
        └── CLAUDE.md
```

## 平台对应关系

| 平台 | 持久规则 | 项目级 Skill | 用户级 Skill |
| --- | --- | --- | --- |
| Cursor | `.cursor/rules/*.mdc` 或 User Rules | `.cursor/skills/<name>/SKILL.md` | `~/.cursor/skills/<name>/SKILL.md` |
| Codex | 项目根目录 `AGENTS.md` 或 `~/.codex/AGENTS.md` | `.agents/skills/<name>/SKILL.md` | `~/.agents/skills/<name>/SKILL.md` |
| Claude Code | 项目根目录 `CLAUDE.md` 或 `~/.claude/CLAUDE.md` | `.claude/skills/<name>/SKILL.md` | `~/.claude/skills/<name>/SKILL.md` |

## 安装到 Cursor

### 项目级 Rule

将 `adapters/cursor/language-logic.mdc` 复制到项目的 `.cursor/rules/`。

```powershell
New-Item -ItemType Directory -Force ".cursor\rules" | Out-Null
Copy-Item "adapters\cursor\language-logic.mdc" ".cursor\rules\language-logic.mdc"
```

### 项目级 Skill

```powershell
New-Item -ItemType Directory -Force ".cursor\skills" | Out-Null
Copy-Item -Recurse "skills\language-output-design" ".cursor\skills\language-output-design"
```

需要在所有项目中使用时，将 Rule 内容添加到 Cursor Settings 的 User Rules，并将 Skill 复制到 `~/.cursor/skills/language-output-design/`。

## 安装到 Codex

Codex 使用 `AGENTS.md` 加载持久指令，并从 `.agents/skills/` 发现项目 Skill。

将 `adapters/codex/AGENTS.md` 的内容合并到项目根目录的 `AGENTS.md`。如果项目没有该文件，可以直接复制。

```powershell
Copy-Item "adapters\codex\AGENTS.md" "AGENTS.md"
New-Item -ItemType Directory -Force ".agents\skills" | Out-Null
Copy-Item -Recurse "skills\language-output-design" ".agents\skills\language-output-design"
```

用户级安装位置如下。

```powershell
New-Item -ItemType Directory -Force "$HOME\.agents\skills" | Out-Null
Copy-Item -Recurse "skills\language-output-design" "$HOME\.agents\skills\language-output-design"
```

需要全局启用 Rule 时，将 `adapters/codex/AGENTS.md` 的内容合并到 `~/.codex/AGENTS.md`。不要直接覆盖已有个人指令。

## 安装到 Claude Code

Claude Code 使用 `CLAUDE.md` 加载持久指令，并从 `.claude/skills/` 发现项目 Skill。

将 `adapters/claude-code/CLAUDE.md` 的内容合并到项目根目录的 `CLAUDE.md`。如果项目没有该文件，可以直接复制。

```powershell
Copy-Item "adapters\claude-code\CLAUDE.md" "CLAUDE.md"
New-Item -ItemType Directory -Force ".claude\skills" | Out-Null
Copy-Item -Recurse "skills\language-output-design" ".claude\skills\language-output-design"
```

用户级安装位置如下。

```powershell
New-Item -ItemType Directory -Force "$HOME\.claude\skills" | Out-Null
Copy-Item -Recurse "skills\language-output-design" "$HOME\.claude\skills\language-output-design"
```

需要全局启用 Rule 时，将 `adapters/claude-code/CLAUDE.md` 的内容合并到 `~/.claude/CLAUDE.md`。不要直接覆盖已有个人指令。

Claude Code 也支持通过 `/language-output-design` 显式调用该 Skill。

## 使用方式

Rule 适用于所有交流。以下任务应按需调用 Skill。

- 教学解释与概念说明
- 技术文档和操作指导
- 故障诊断与证据组织
- 方案比较与决策建议
- 总结、审阅和语言改写
- 实验效果、参数和规格比较

Skill 的主文件会根据任务将 Agent 引导到最少的必要资料。不要预读整个 `references/` 目录。

## 设计原则

- 先确定受众真正需要的行动、判断、机制、记忆框架或可查事实
- 选择能够组织后文的认知支点，不机械规定先给结论或先讲原理
- 先完成主线，再展开不会改变结论的支线
- 连续关系使用段落，独立项目使用列表，共同维度的横向比较使用表格
- 对新手补充必要关系，对熟手保留差异、边界和新增信息
- 把决定性限制、安全风险和不可逆后果放在相关动作之前
- 用机制、条件、证据和结果替代空泛总结与同义重复

## 兼容性依据

- [Cursor Rules](https://docs.cursor.com/context/rules)
- [Cursor Agent Skills](https://docs.cursor.com/context/skills)
- [Codex AGENTS.md](https://developers.openai.com/codex/guides/agents-md)
- [Codex Skills](https://developers.openai.com/codex/codex-manual.md)
- [Claude Code Skills](https://docs.anthropic.com/en/docs/claude-code/skills)
- [Claude Code extension overview](https://docs.anthropic.com/en/docs/claude-code/features-overview)
- [Agent Skills open standard](https://agentskills.io/)

## License

MIT
