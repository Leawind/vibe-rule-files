| 中文 | [English](README.md) |
| ---- | -------------------- |

# Rule Files of Vibe Coding Tools

## 总览

| 工具           | 项目规则文件                                                        | 备注                                                      |
| -------------- | ------------------------------------------------------------------- | --------------------------------------------------------- |
| Claude Code    | `CLAUDE.md`、`.claude/rules/*.md`                                   | 支持 `@import` 语法导入其他文件                           |
| Cline          | `.clinerules/*.md`                                                  | 同时兼容 `.cursorrules`、`.windsurfrules`、`AGENTS.md`    |
| CodeBuddy      | `CODEBUDDY.md`、`.codebuddy/rules/*/RULE.mdc`                       | 规则类型分为 Always、Agent Requested、Manual              |
| Cursor         | `.cursor/rules/*.mdc`、`AGENTS.md`                                  | `.mdc` 文件支持 frontmatter 控制触发方式                  |
| Gemini CLI     | `GEMINI.md`                                                         | 支持 `@file.md` 语法导入子文件                            |
| GitHub Copilot | `.github/copilot-instructions.md`、`AGENTS.md`                      | 路径特定规则放在 `.github/instructions/*.instructions.md` |
| Hermes Agent   | `.hermes.md`、`HERMES.md`、`AGENTS.md`、`CLAUDE.md`、`.cursorrules` | 支持渐进式子目录发现；`SOUL.md` 用于全局个性定制          |
| Kilo Code      | `.kilo/rules/*.md` (推荐)                                           | 通过 `kilo.jsonc` 配置指令路径，兼容旧版 `.kilocode/`     |
| MiMo Code      | `AGENTS.md`、`CLAUDE.md`                                            | 可在 `mimocode.json` 中自定义指令文件路径                 |
| OpenCode       | `AGENTS.md`                                                         | 可在 `opencode.json` 中自定义指令文件路径                 |
| Trae           | `.trae/rules/*.md`                                                  | 支持 frontmatter 控制触发场景（如 `scene: git_message`）  |

## 工具

### [Claude Code](https://claude.ai/code)

> 文档: <https://docs.anthropic.com/en/docs/claude-code/memory>

项目规则文件:

- 主要: 项目根目录的 `CLAUDE.md` 或 `.claude/CLAUDE.md`
- 细分规则: `.claude/rules/*.md` — 可按文件路径或主题拆分
- 个人偏好: `CLAUDE.local.md` — 加入 `.gitignore`，不提交到版本控制

层级加载顺序 (从宽到窄):

1. 组织级: `/etc/claude-code/CLAUDE.md`
2. 用户级: `~/.claude/CLAUDE.md`
3. 项目级: `./CLAUDE.md` 或 `./.claude/CLAUDE.md`
4. 本地级: `./CLAUDE.local.md`

### [Cline](https://cline.bot)

> 文档: <https://docs.cline.bot/features/cline-rules>

项目规则文件:

- 主要: `.clinerules/*.md` — 支持 `.md` 和 `.txt` 文件
- 自动兼容: 也会识别 `.cursorrules`、`.windsurfrules`、`AGENTS.md`
- 全局规则: `~/Documents/Cline/Rules/`

### [CodeBuddy](https://www.codebuddy.ai)

> 文档: <https://www.codebuddy.ai/docs/ide/Rules>

项目规则文件:

- 主文件: 项目根目录的 `CODEBUDDY.md` — 提供项目上下文和全局指令
- 细分规则: `.codebuddy/rules/*/RULE.mdc` — 每个规则对应一个包含 `RULE.mdc`
  的文件夹
- 全局规则: `~/.codebuddy/CODEBUDDY.md` 及 `~/.codebuddy/rules/`

规则类型:

| 类型              | 说明                      |
| ----------------- | ------------------------- |
| `Always`          | 始终生效                  |
| `Agent Requested` | AI 根据上下文自行判断启用 |
| `Manual`          | 仅在手动指定时生效        |

### [Cursor](https://cursor.com)

> 文档: <https://cursor.com/docs/rules>

项目规则文件:

- 主要: `.cursor/rules/*.mdc` — 使用 `.mdc` 扩展名，通过 frontmatter
  控制触发方式
- 简单替代: 项目根目录的 `AGENTS.md` — 纯 Markdown，无需配置

规则类型 (通过 frontmatter 控制):

| 类型                    | 说明                        |
| ----------------------- | --------------------------- |
| `alwaysApply: true`     | 每次对话都生效              |
| `globs: "src/**/*.tsx"` | 匹配特定文件时自动生效      |
| `description: "..."`    | AI 根据描述自行判断是否启用 |
| 手动 `@rule-name`       | 仅在聊天中 @提及时生效      |

### [Gemini CLI](https://geminicli.com)

> 文档: <https://geminicli.com/docs/cli/gemini-md>

项目规则文件:

- 主要: 项目根目录的 `GEMINI.md`
- 子目录: 任意子目录中的 `GEMINI.md` — 仅在访问该目录时加载
- 全局: `~/.gemini/GEMINI.md`

层级加载顺序:

1. 全局: `~/.gemini/GEMINI.md`
2. 工作区: 从当前目录向上搜索 `GEMINI.md`
3. 即时加载 (JIT): 访问文件/目录时自动加载该目录的 `GEMINI.md`

支持导入语法: `@path/to/file.md`

### [GitHub Copilot](https://github.com/features/copilot)

> 文档: <https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot>

项目规则文件:

- 仓库级: `.github/copilot-instructions.md` — 对整个仓库生效
- 路径特定: `.github/instructions/*.instructions.md` — 文件名必须以
  `.instructions.md` 结尾
- Agent 指令: 项目根目录的 `AGENTS.md`（或 `CLAUDE.md`、`GEMINI.md`）

### [Hermes Agent](https://hermes-agent.nousresearch.com)

> 文档: <https://hermes-agent.nousresearch.com/docs/user-guide/features/context-files>

项目规则文件:

- 主要: `.hermes.md` 或 `HERMES.md` — 最高优先级的项目指令
- 兼容: `AGENTS.md`、`CLAUDE.md`、`.cursorrules` — 同样会被检测和加载
- 全局个性: `~/.hermes/SOUL.md` — 控制代理的语气和沟通风格
- Cursor 规则: `.cursor/rules/*.mdc` — Cursor IDE 规则模块（如果没有更高优先级的文件）

优先级系统 (第一个匹配的生效):

1. `.hermes.md` / `HERMES.md`
2. `AGENTS.md`
3. `CLAUDE.md`
4. `.cursorrules`

特性:

- 渐进式子目录发现: 导航到子目录时会自动加载其中的 `AGENTS.md` 文件
- 安全扫描: 所有上下文文件都会检查提示注入模式
- 大小限制: 超过 20,000 字符的文件会被截断（70% 头部，20% 尾部）

### [Kilo Code](https://kilo.ai)

> 文档: <https://kilo.ai/docs/customize/custom-rules>

项目规则文件:

- 推荐配置: 在 `kilo.jsonc` 中通过 `instructions` 数组指定路径，默认推荐 `.kilo/rules/*.md`
- 兼容路径: `.kilocode/rules/*.md` — 旧版路径，仍被支持但不推荐新建项目使用
- 全局规则: 在 `~/.config/kilo/kilo.jsonc` 中配置

规则说明:

- 支持通过规则统一代码格式化标准、限制敏感文件访问及强制执行编码规范
- 建议通过配置文件显式管理不同用途的规则文件，而非依赖自动识别的模式目录

### [MiMo Code](https://mimo.xiaomi.com/mimocode)

> 文档: <https://mimo.xiaomi.com/zh/mimocode/rules>

项目规则文件:

- 主要: `AGENTS.md` 或兼容的 `CLAUDE.md` — 作为项目级规则文件
- 配置文件: `mimocode.json` 或全局 `~/.config/mimocode/mimocode.json`

配置说明:

- 可在 JSON 配置文件中指定自定义指令文件路径，复用现有规则
- 全局规则文件 (`~/.config/mimocode/AGENTS.md`) 对所有项目会话生效，实现跨项目指令持久化

### [OpenCode](https://opencode.ai)

> 文档: <https://opencode.ai/docs/rules/>

项目规则文件:

- 主要: 项目根目录的 `AGENTS.md` — 提供自定义指令
- 配置文件: `opencode.json` 或全局 `~/.config/opencode/opencode.json`

配置说明:

- 可在 `opencode.json` 中指定自定义指令文件的路径，复用现有规则而无需复制到
  `AGENTS.md`
- 支持通过 `/init` 命令快速生成或更新 `AGENTS.md` 文件

### [Trae](https://trae.ai)

> 文档: <https://docs.trae.ai/ide/rules>

项目规则文件:

- 主要: `.trae/rules/*.md` — 存放于项目根目录的隐藏目录下
- 个人规则: 在 IDE 设置中配置，对所有项目全局生效

规则类型 (通过 frontmatter 控制):

| 类型                 | 说明                            |
| -------------------- | ------------------------------- |
| `alwaysApply: true`  | 每次对话都生效                  |
| `scene: git_message` | 仅在生成 Git 提交信息时自动生效 |
| 手动触发             | 在聊天中通过 `@` 提及时生效     |
