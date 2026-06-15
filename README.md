| [中文](README.zh.md) | English |
| -------------------- | ------- |

# Rule Files of Vibe Coding Tools

## Overview

| Tool           | Project Rule Files                             | Notes                                                                             |
| -------------- | ---------------------------------------------- | --------------------------------------------------------------------------------- |
| Claude Code    | `CLAUDE.md`, `.claude/rules/*.md`              | Supports `@import` syntax to include other files                                  |
| Cline          | `.clinerules/*.md`                             | Also compatible with `.cursorrules`, `.windsurfrules`, `AGENTS.md`                |
| CodeBuddy      | `CODEBUDDY.md`, `.codebuddy/rules/*/RULE.mdc`  | Rule types: Always, Agent Requested, Manual                                       |
| Cursor         | `.cursor/rules/*.mdc`, `AGENTS.md`             | `.mdc` files support frontmatter to control triggering                            |
| Gemini CLI     | `GEMINI.md`                                    | Supports `@file.md` syntax to import subfiles                                     |
| GitHub Copilot | `.github/copilot-instructions.md`, `AGENTS.md` | Path-specific rules placed in `.github/instructions/*.instructions.md`            |
| Kilo Code      | `.kilo/rules/*.md` (recommended)               | Configure instruction paths via `kilo.jsonc`; compatible with legacy `.kilocode/` |
| MiMo Code      | `AGENTS.md`, `CLAUDE.md`                       | Customizable instruction file paths in `mimocode.json`                            |
| OpenCode       | `AGENTS.md`                                    | Customizable instruction file paths in `opencode.json`                            |
| Trae           | `.trae/rules/*.md`                             | Supports frontmatter to control trigger scenarios (e.g., `scene: git_message`)    |

## Tools

### [Claude Code](https://claude.ai/code)

> Documentation: <https://docs.anthropic.com/en/docs/claude-code/memory>

Project Rule Files:

- Primary: `CLAUDE.md` or `.claude/CLAUDE.md` in the project root
- Granular Rules: `.claude/rules/*.md` — can be split by file path or topic
- Personal Preferences: `CLAUDE.local.md` — add to `.gitignore`, do not commit to version control

Hierarchy Loading Order (from broad to narrow):

1. Organization Level: `/etc/claude-code/CLAUDE.md`
2. User Level: `~/.claude/CLAUDE.md`
3. Project Level: `./CLAUDE.md` or `./.claude/CLAUDE.md`
4. Local Level: `./CLAUDE.local.md`

### [Cline](https://cline.bot)

> Documentation: <https://docs.cline.bot/features/cline-rules>

Project Rule Files:

- Primary: `.clinerules/*.md` — supports `.md` and `.txt` files
- Auto-Compatible: Also recognizes `.cursorrules`, `.windsurfrules`, `AGENTS.md`
- Global Rules: `~/Documents/Cline/Rules/`

### [CodeBuddy](https://www.codebuddy.ai)

> Documentation: <https://www.codebuddy.ai/docs/ide/Rules>

Project Rule Files:

- Main File: `CODEBUDDY.md` in the project root — provides project context and global instructions
- Granular Rules: `.codebuddy/rules/*/RULE.mdc` — each rule corresponds to a folder containing `RULE.mdc`
- Global Rules: `~/.codebuddy/CODEBUDDY.md` and `~/.codebuddy/rules/`

Rule Types:

| Type              | Description                                   |
| ----------------- | --------------------------------------------- |
| `Always`          | Always active                                 |
| `Agent Requested` | AI decides whether to enable based on context |
| `Manual`          | Active only when manually specified           |

### [Cursor](https://cursor.com)

> Documentation: <https://cursor.com/docs/rules>

Project Rule Files:

- Primary: `.cursor/rules/*.mdc` — uses `.mdc` extension; controls triggering via frontmatter
- Simple Alternative: `AGENTS.md` in the project root — plain Markdown, no configuration required

Rule Types (controlled via frontmatter):

| Type                    | Description                                       |
| ----------------------- | ------------------------------------------------- |
| `alwaysApply: true`     | Active in every conversation                      |
| `globs: "src/**/*.tsx"` | Automatically active when matching specific files |
| `description: "..."`    | AI decides whether to enable based on description |
| Manual `@rule-name`     | Active only when mentioned with `@` in chat       |

### [Gemini CLI](https://geminicli.com)

> Documentation: <https://geminicli.com/docs/cli/gemini-md>

Project Rule Files:

- Primary: `GEMINI.md` in the project root
- Subdirectories: `GEMINI.md` in any subdirectory — loaded only when accessing that directory
- Global: `~/.gemini/GEMINI.md`

Hierarchy Loading Order:

1. Global: `~/.gemini/GEMINI.md`
2. Workspace: Searches upward from the current directory for `GEMINI.md`
3. Just-In-Time (JIT): Automatically loads `GEMINI.md` of the directory when accessing files/directories

Supports import syntax: `@path/to/file.md`

### [GitHub Copilot](https://github.com/features/copilot)

> Documentation: <https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot>

Project Rule Files:

- Repository Level: `.github/copilot-instructions.md` — applies to the entire repository
- Path-Specific: `.github/instructions/*.instructions.md` — filenames must end with `.instructions.md`
- Agent Instructions: `AGENTS.md` (or `CLAUDE.md`, `GEMINI.md`) in the project root

### [Kilo Code](https://kilo.ai)

> Documentation: <https://kilo.ai/docs/customize/custom-rules>

Project Rule Files:

- Recommended Configuration: Specify paths via the `instructions` array in `kilo.jsonc`; default recommendation is `.kilo/rules/*.md`
- Compatible Paths: `.kilocode/rules/*.md` — legacy path, still supported but not recommended for new projects
- Global Rules: Configured in `~/.config/kilo/kilo.jsonc`

Rule Description:

- Supports unifying code formatting standards, restricting access to sensitive files, and enforcing coding norms through rules
- It is recommended to explicitly manage rule files for different purposes via configuration files, rather than relying on automatically recognized pattern directories

### [MiMo Code](https://mimo.xiaomi.com/mimocode)

> Documentation: <https://mimo.xiaomi.com/zh/mimocode/rules>

Project Rule Files:

- Primary: `AGENTS.md` or compatible `CLAUDE.md` — serves as project-level rule files
- Configuration File: `mimocode.json` or global `~/.config/mimocode/mimocode.json`

Configuration Description:

- Custom instruction file paths can be specified in the JSON configuration file to reuse existing rules
- Global rule files (`~/.config/mimocode/AGENTS.md`) apply to all project sessions, enabling cross-project instruction persistence

### [OpenCode](https://opencode.ai)

> Documentation: <https://opencode.ai/docs/rules/>

Project Rule Files:

- Primary: `AGENTS.md` in the project root — provides custom instructions
- Configuration File: `opencode.json` or global `~/.config/opencode/opencode.json`

Configuration Description:

- Custom instruction file paths can be specified in `opencode.json` to reuse existing rules without copying them to `AGENTS.md`
- Supports quickly generating or updating `AGENTS.md` files via the `/init` command

### [Trae](https://trae.ai)

> Documentation: <https://docs.trae.ai/ide/rules>

Project Rule Files:

- Primary: `.trae/rules/*.md` — stored in a hidden directory in the project root
- Personal Rules: Configured in IDE settings, applying globally to all projects

Rule Types (controlled via frontmatter):

| Type                 | Description                                                   |
| -------------------- | ------------------------------------------------------------- |
| `alwaysApply: true`  | Active in every conversation                                  |
| `scene: git_message` | Automatically active only when generating Git commit messages |
| Manual Trigger       | Active when mentioned with `@` in chat                        |
