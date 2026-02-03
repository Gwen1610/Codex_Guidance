# `config-reference-guide.md` 中文阐释

本文是对 `config/config-reference-guide.md` 的中文解读，目标是把“字段清单”变成可读、可用的配置理解。内容基于官方 Config Reference 与 Sample Configuration。建议先阅读本文件，再按需回到字段清单逐条查键名。

## 如何使用这份解读

1. 先确定你的使用场景：个人本地、团队协作、公司内网、或需要严格审批的环境。
2. 根据场景阅读对应模块（模型、审批、沙箱、MCP、Skills、TUI 等）。
3. 回到 `config-reference-guide.md` 找到具体 Key，复制到 `config.toml` 中。

## 配置文件位置与作用域

- 用户级配置：`~/.codex/config.toml`（所有项目通用）。
- 项目级覆盖：`.codex/config.toml`（仅在你“信任项目”后才会被加载）。
- 这意味着：团队项目可以有局部配置，但不会在你未确认信任前生效。该机制避免陌生项目偷偷改变你的运行策略。citeturn0search2

## 模型与推理相关

核心目标：确定用哪个模型、上下文长度、推理强度。

- `model` 是主模型，`review_model` 只影响 `/review`。
- `model_provider` 指向 `model_providers` 中的供应商 id。
- `model_context_window`、`model_auto_compact_token_limit` 控制上下文上限与自动压缩阈值。
- 推理相关：`model_reasoning_effort`、`model_reasoning_summary`、`model_verbosity`。
- 若你只需要“能跑起来”，保持默认即可；若经常长对话或多文件推理，再考虑调整上下文和压缩阈值。citeturn0search1turn0search2

## 审批与沙箱

核心目标：在“效率”与“安全”之间找平衡。

- `approval_policy` 决定何时需要人工批准命令。
- `sandbox_mode` 决定命令是否有写权限、是否允许联网。
- 如果是个人学习环境，可以用 `workspace-write`；如果是团队或重要环境，建议保守选择并开启审批。citeturn0search1turn0search2

## 指令注入与压缩策略

核心目标：控制 Codex 的基础行为与历史压缩方式。

- `developer_instructions`：注入你的额外约束。
- `model_instructions_file` 或 `experimental_instructions_file`：用文件替换内置指令。
- `compact_prompt` 或 `experimental_compact_prompt_file`：自定义压缩提示词。
- 若你遇到上下文被压缩导致信息丢失，可考虑调整压缩阈值或定制压缩提示词。citeturn0search1turn0search2

## Skills 与 MCP 扩展

核心目标：为 Codex 增加“专业能力”或“外部工具能力”。

- Skills：通过 `skills.config` 启用或禁用某些技能。
- MCP：通过 `mcp_servers.<id>.*` 配置本地或远程工具服务。
- 建议先把稳定、可信的工具接入，再逐步扩展。citeturn0search2

## 终端体验（TUI）

核心目标：优化你在终端里的使用体验。

- `tui.*` 控制动画、通知方式、提示信息等。
- 初学者默认即可；如果你觉得提示打扰或希望更静默，可关掉通知或动画。citeturn0search2

## Web Search

核心目标：是否允许 Codex 进行网络搜索，以及搜索模式。

- `web_search = disabled | cached | live`。
- `cached` 使用维护的索引，`live` 会抓取实时网页。
- 如果工作环境严格禁止外网访问，建议设为 `disabled`。citeturn0search2

## 登录与凭据

核心目标：管理登录方式与凭据存储位置。

- `cli_auth_credentials_store` 控制 CLI 凭据存储在文件或系统钥匙串。
- `chatgpt_base_url` 与 `forced_login_method` 用于限制登录流程。
- `mcp_oauth_credentials_store` 控制 MCP OAuth 的存储方式。citeturn0search1turn0search2

## 项目文档控制

核心目标：控制 `AGENTS.md` 等项目文档注入规模与回退策略。

- `project_doc_max_bytes` 控制注入大小上限。
- `project_doc_fallback_filenames` 用于找不到 `AGENTS.md` 时的备用文件名。

## 实验与兼容性开关

核心目标：控制一些实验/兼容行为，通常不建议新手随意修改。

- `features.*` 里包含实验功能和旧功能的替代开关。
- 带有 `experimental_` 前缀的一般属于旧路径或试验功能。

## 最后建议

- 不要一次性写入所有字段，只保留“你真正需要改动”的那几项。
- 写完后可参考 `config.sample.toml` 对照检查格式是否正确。citeturn0search1
