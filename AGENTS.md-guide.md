不是一个东西，但**用途很像**：都属于“给 agent 的项目级/层级化指令（memory / rules）”，让它更懂你的项目和做事方式。

**主要区别**
- `CLAUDE.md`：是 Anthropic Claude Code 的“memory files”。Claude Code 启动时会自动加载这些记忆文件，并且支持分层与导入（`@path`）。([docs.anthropic.com](https://docs.anthropic.com/en/docs/claude-code/memory?utm_source=openai))  
- `AGENTS.md`：是 OpenAI Codex 的“agents instructions”。Codex 会读取层级化的 `AGENTS.md`（以及全局的 `~/.codex/AGENTS.md`）来决定在该目录下如何工作。([docs.anthropic.com](https://docs.anthropic.com/en/docs/claude-code/settings?utm_source=openai))

**是否“同样可以放项目核心框架和理念”？**
- 可以，思路一致：写“架构概览 + 约定 + 常用命令 + 禁区”。但注意别写太多泛信息；在 Claude Code 里官方也提醒 memory 的内容应偏“通用且经常用到”，否则可能被模型判断为不相关而不生效。([docs.anthropic.com](https://docs.anthropic.com/en/docs/claude-code/memory?utm_source=openai))

**是否需要每个新目录都建一个？**
- 不需要。通常：
  - 全局偏好放 `~/.codex/AGENTS.md`
  - 项目偏好放项目根目录 `AGENTS.md`
  - 子目录只有在需要“覆盖/补充规则”时才加

如果你愿意，我可以给你一个“最小但高效”的 `AGENTS.md` 模板（适合你现在这个 repo），你再把“核心框架/理念”按模块填进去。

