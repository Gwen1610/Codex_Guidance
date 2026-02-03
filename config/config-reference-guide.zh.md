# `config-reference-guide.md` 中文阐释

本文是对 `config/config-reference-guide.md` 的中文解读，目标是把“字段清单”变成可读、可用的配置理解。内容基于官方 Config Reference 与 Sample Configuration。建议先阅读本文件，再按需回到字段清单逐条查键名。

## 如何使用这份解读

1. 先确定你的使用场景：个人本地、团队协作、公司内网、或需要严格审批的环境。
2. 根据场景阅读对应模块（模型、审批、沙箱、MCP、Skills、TUI 等）。
3. 回到 `config-reference-guide.md` 找到具体 Key，复制到 `config.toml` 中。

## 配置文件位置与作用域

- 用户级配置：`~/.codex/config.toml`
- 项目级覆盖：`.codex/config.toml`（只有在你信任该项目时才会加载）

## 逐条 Key 对照表（中文解释）

说明：中文解释为“功能归类 + 简要说明”，如需精确信息请以官方英文说明为准。

| Key | Type / Values | 官方说明 | 中文解释 |
| --- | --- | --- | --- |
| `model` | `string` | Model to use (e.g., `gpt-5-codex`). | 指定默认使用的模型。 |
| `review_model` | `string` | Optional model override used by `/review` (defaults to the current session model). | 为 `/review` 指定单独的模型（可选）。 |
| `model_provider` | `string` | Provider id from `model_providers` (default: `openai`). | 选择模型供应商（对应 `model_providers` 的 id）。 |
| `model_context_window` | `number` | Context window tokens available to the active model. | 设置当前模型可用的上下文 token 上限。 |
| `model_auto_compact_token_limit` | `number` | Token threshold that triggers automatic history compaction (unset uses model defaults). | 触发自动压缩历史的 token 阈值。 |
| `oss_provider` | `lmstudio | ollama` | Default local provider used when running with `--oss` (defaults to prompting if unset). | 在 `--oss` 场景下默认使用的本地模型提供商。 |
| `approval_policy` | `untrusted | on-failure | on-request | never` | Controls when Codex pauses for approval before executing commands. | 控制何时需要人工批准命令执行。 |
| `sandbox_mode` | `read-only | workspace-write | danger-full-access` | Sandbox policy for filesystem and network access during command execution. | 命令执行的沙箱权限等级。 |
| `sandbox_workspace_write.writable_roots` | `array<string>` | Additional writable roots when `sandbox_mode = \"workspace-write\"`. | workspace-write 模式的额外可写目录。 |
| `sandbox_workspace_write.network_access` | `boolean` | Allow outbound network access inside the workspace-write sandbox. | workspace-write 模式下允许外网访问。 |
| `sandbox_workspace_write.exclude_tmpdir_env_var` | `boolean` | Exclude `$TMPDIR` from writable roots in workspace-write mode. | 排除 $TMPDIR 的可写权限。 |
| `sandbox_workspace_write.exclude_slash_tmp` | `boolean` | Exclude `/tmp` from writable roots in workspace-write mode. | 排除 /tmp 的可写权限。 |
| `notify` | `array<string>` | Command invoked for notifications; receives a JSON payload from Codex. | 通知回调命令（接收 Codex 的 JSON 事件）。 |
| `check_for_update_on_startup` | `boolean` | Check for Codex updates on startup (set to false only when updates are centrally managed). | 是否在启动时检查更新。 |
| `feedback.enabled` | `boolean` | Enable feedback submission via `/feedback` across Codex surfaces (default: true). | 是否启用 `/feedback` 反馈提交通道。 |
| `instructions` | `string` | Reserved for future use; prefer `model_instructions_file` or `AGENTS.md`. | 保留字段，建议使用 `model_instructions_file` 或 `AGENTS.md`。 |
| `developer_instructions` | `string` | Additional developer instructions injected into the session (optional). | 注入额外的开发者指令。 |
| `compact_prompt` | `string` | Inline override for the history compaction prompt. | 自定义历史压缩提示词。 |
| `model_instructions_file` | `string (path)` | Replacement for built-in instructions instead of `AGENTS.md`. | 用文件替换内置指令（代替 `AGENTS.md`）。 |
| `experimental_compact_prompt_file` | `string (path)` | Load the compaction prompt override from a file (experimental). | 从文件加载压缩提示词（实验）。 |
| `skills.config` | `array<object>` | Per-skill enablement overrides stored in config.toml. | 配置项说明：参见官方描述。 |
| `skills.config.<index>.path` | `string (path)` | Path to a skill folder containing `SKILL.md`. | 技能目录路径（包含 `SKILL.md`）。 |
| `skills.config.<index>.enabled` | `boolean` | Enable or disable the referenced skill. | 是否启用该技能。 |
| `mcp_servers.<id>.command` | `string` | Launcher command for an MCP stdio server. | MCP stdio 服务器启动命令。 |
| `mcp_servers.<id>.args` | `array<string>` | Arguments passed to the MCP stdio server command. | MCP stdio 服务器启动参数。 |
| `mcp_servers.<id>.env` | `map<string,string>` | Environment variables forwarded to the MCP stdio server. | MCP 服务器的环境变量映射。 |
| `mcp_servers.<id>.env_vars` | `array<string>` | Additional environment variables to whitelist for an MCP stdio server. | MCP 服务器允许的环境变量白名单。 |
| `mcp_servers.<id>.cwd` | `string` | Working directory for the MCP stdio server process. | MCP 服务器工作目录。 |
| `mcp_servers.<id>.url` | `string` | Endpoint for an MCP streamable HTTP server. | MCP HTTP 服务器地址。 |
| `mcp_servers.<id>.bearer_token_env_var` | `string` | Environment variable sourcing the bearer token for an MCP HTTP server. | MCP HTTP 的 Bearer Token 环境变量名。 |
| `mcp_servers.<id>.http_headers` | `map<string,string>` | Static HTTP headers included with each MCP HTTP request. | MCP HTTP 请求的静态请求头。 |
| `mcp_servers.<id>.env_http_headers` | `map<string,string>` | HTTP headers populated from environment variables for an MCP HTTP server. | 来自环境变量的 MCP HTTP 请求头。 |
| `mcp_servers.<id>.enabled` | `boolean` | Disable an MCP server without removing its configuration. | 是否启用该 MCP 服务器。 |
| `mcp_servers.<id>.startup_timeout_sec` | `number` | Override the default 10s startup timeout for an MCP server. | MCP 服务器启动超时设置。 |
| `mcp_servers.<id>.startup_timeout_ms` | `number` | Alias for `startup_timeout_sec` in milliseconds. | MCP 服务器启动超时设置。 |
| `mcp_servers.<id>.tool_timeout_sec` | `number` | Override the default 60s per-tool timeout for an MCP server. | MCP 单工具超时设置。 |
| `mcp_servers.<id>.enabled_tools` | `array<string>` | Allow list of tool names exposed by the MCP server. | MCP 允许使用的工具白名单。 |
| `mcp_servers.<id>.disabled_tools` | `array<string>` | Deny list applied after `enabled_tools` for the MCP server. | MCP 禁止使用的工具黑名单。 |
| `features.unified_exec` | `boolean` | Use the unified PTY-backed exec tool (beta). | 启用统一的 PTY 执行工具（beta）。 |
| `features.shell_snapshot` | `boolean` | Snapshot shell environment to speed up repeated commands (beta). | 启用 shell 环境快照（beta）。 |
| `features.apply_patch_freeform` | `boolean` | Expose the freeform `apply_patch` tool (experimental). | 启用自由格式 `apply_patch`（实验）。 |
| `features.web_search` | `boolean` | Deprecated legacy toggle; prefer the top-level `web_search` setting. | 旧版 web_search 开关（已弃用）。 |
| `features.web_search_cached` | `boolean` | Deprecated legacy toggle. When `web_search` is unset, true maps to `web_search = \"cached\"`. | 旧版 web_search 缓存开关（已弃用）。 |
| `features.web_search_request` | `boolean` | Deprecated legacy toggle. When `web_search` is unset, true maps to `web_search = \"live\"`. | 旧版 web_search 实时开关（已弃用）。 |
| `features.shell_tool` | `boolean` | Enable the default `shell` tool for running commands (stable; on by default). | 是否启用默认 shell 工具。 |
| `features.request_rule` | `boolean` | Enable Smart approvals (`prefix_rule` suggestions on escalation requests; stable; on by default). | 启用智能审批规则提示。 |
| `features.exec_policy` | `boolean` | Enforce rules checks for `shell`/`unified_exec` (experimental; on by default). | 启用命令执行策略校验。 |
| `features.experimental_windows_sandbox` | `boolean` | Run the Windows restricted-token sandbox (experimental). | 启用 Windows 受限令牌沙箱（实验）。 |
| `features.elevated_windows_sandbox` | `boolean` | Enable the elevated Windows sandbox pipeline (experimental). | 启用 Windows 提权沙箱（实验）。 |
| `features.remote_compaction` | `boolean` | Enable remote compaction (ChatGPT auth only; experimental; on by default). | 启用远程压缩（实验）。 |
| `features.remote_models` | `boolean` | Refresh remote model list before showing readiness (experimental). | 启动时刷新远程模型列表（实验）。 |
| `features.powershell_utf8` | `boolean` | Force PowerShell UTF-8 output (defaults to true). | 强制 PowerShell UTF-8 输出。 |
| `features.child_agents_md` | `boolean` | Append AGENTS.md scope/precedence guidance even when no AGENTS.md is present (experimental). | 无 AGENTS.md 时附加作用域提示（实验）。 |
| `suppress_unstable_features_warning` | `boolean` | Suppress the warning that appears when under-development feature flags are enabled. | 隐藏不稳定功能的提示警告。 |
| `model_providers.<id>.name` | `string` | Display name for a custom model provider. | 自定义模型供应商的显示名。 |
| `model_providers.<id>.base_url` | `string` | API base URL for the model provider. | 自定义模型供应商的 API 地址。 |
| `model_providers.<id>.env_key` | `string` | Environment variable supplying the provider API key. | 供应商 API Key 的环境变量名。 |
| `model_providers.<id>.env_key_instructions` | `string` | Optional setup guidance for the provider API key. | API Key 的配置说明。 |
| `model_providers.<id>.experimental_bearer_token` | `string` | Direct bearer token for the provider (discouraged; use `env_key`). | 直接设置 Bearer Token（不推荐）。 |
| `model_providers.<id>.requires_openai_auth` | `boolean` | The provider uses OpenAI authentication (defaults to false). | 该供应商是否需要 OpenAI 认证。 |
| `model_providers.<id>.wire_api` | `chat | responses` | Protocol used by the provider (defaults to `chat` if omitted). | 供应商使用的协议类型（chat/responses）。 |
| `model_providers.<id>.query_params` | `map<string,string>` | Extra query parameters appended to provider requests. | 追加到请求的查询参数。 |
| `model_providers.<id>.http_headers` | `map<string,string>` | Static HTTP headers added to provider requests. | 追加到请求的静态请求头。 |
| `model_providers.<id>.env_http_headers` | `map<string,string>` | HTTP headers populated from environment variables when present. | 从环境变量读取的请求头。 |
| `model_providers.<id>.request_max_retries` | `number` | Retry count for HTTP requests to the provider (default: 4). | 普通请求的最大重试次数。 |
| `model_providers.<id>.stream_max_retries` | `number` | Retry count for SSE streaming interruptions (default: 5). | 流式请求中断后的最大重试次数。 |
| `model_providers.<id>.stream_idle_timeout_ms` | `number` | Idle timeout for SSE streams in milliseconds (default: 300000). | 流式请求空闲超时。 |
| `model_reasoning_effort` | `minimal | low | medium | high | xhigh` | Adjust reasoning effort for supported models (Responses API only; `xhigh` is model-dependent). | 控制推理强度（适用于支持的模型）。 |
| `model_reasoning_summary` | `auto | concise | detailed | none` | Select reasoning summary detail or disable summaries entirely. | 控制推理摘要的详细程度或关闭摘要。 |
| `model_verbosity` | `low | medium | high` | Control GPT-5 Responses API verbosity (defaults to `medium`). | 控制回答的详细程度（仅部分模型/协议）。 |
| `model_supports_reasoning_summaries` | `boolean` | Force Codex to send reasoning metadata even for unknown models. | 强制为未知模型开启推理摘要支持。 |
| `shell_environment_policy.inherit` | `all | core | none` | Baseline environment inheritance when spawning subprocesses. | 子进程继承环境变量的基线策略。 |
| `shell_environment_policy.ignore_default_excludes` | `boolean` | Keep variables containing KEY/SECRET/TOKEN before other filters run. | 保留包含 KEY/SECRET/TOKEN 的变量。 |
| `shell_environment_policy.exclude` | `array<string>` | Glob patterns for removing environment variables after the defaults. | 排除匹配列表的环境变量。 |
| `shell_environment_policy.include_only` | `array<string>` | Whitelist of patterns; when set only matching variables are kept. | 仅保留匹配列表的环境变量。 |
| `shell_environment_policy.set` | `map<string,string>` | Explicit environment overrides injected into every subprocess. | 为子进程注入的固定环境变量。 |
| `shell_environment_policy.experimental_use_profile` | `boolean` | Use the user shell profile when spawning subprocesses. | 使用用户 shell 配置文件（实验）。 |
| `project_root_markers` | `array<string>` | List of project root marker filenames; used when searching parent directories for the project root. | 配置项说明：参见官方描述。 |
| `project_doc_max_bytes` | `number` | Maximum bytes read from `AGENTS.md` when building project instructions. | 项目文档注入的最大字节数。 |
| `project_doc_fallback_filenames` | `array<string>` | Additional filenames to try when `AGENTS.md` is missing. | 找不到 `AGENTS.md` 时的备用文件名列表。 |
| `profile` | `string` | Default profile applied at startup (equivalent to `--profile`). | 配置项说明：参见官方描述。 |
| `profiles.<name>.*` | `various` | Profile-scoped overrides for any of the supported configuration keys. | 配置 profile 相关设置。 |
| `profiles.<name>.include_apply_patch_tool` | `boolean` | Legacy name for enabling freeform apply_patch; prefer `[features].apply_patch_freeform`. | 配置 profile 相关设置。 |
| `profiles.<name>.web_search` | `disabled | cached | live` | Profile-scoped web search mode override (default: `\"cached\"`). | 配置 profile 相关设置。 |
| `profiles.<name>.experimental_use_unified_exec_tool` | `boolean` | Legacy name for enabling unified exec; prefer `[features].unified_exec`. | 配置 profile 相关设置。 |
| `profiles.<name>.experimental_use_freeform_apply_patch` | `boolean` | Legacy name for enabling freeform apply_patch; prefer `[features].apply_patch_freeform`. | 配置 profile 相关设置。 |
| `profiles.<name>.oss_provider` | `lmstudio | ollama` | Profile-scoped OSS provider for `--oss` sessions. | 配置 profile 相关设置。 |
| `history.persistence` | `save-all | none` | Control whether Codex saves session transcripts to history.jsonl. | 配置项说明：参见官方描述。 |
| `tool_output_token_limit` | `number` | Token budget for storing individual tool/function outputs in history. | 单次工具输出写入历史的 token 上限。 |
| `history.max_bytes` | `number` | If set, caps the history file size in bytes by dropping oldest entries. | 配置项说明：参见官方描述。 |
| `file_opener` | `vscode | vscode-insiders | windsurf | cursor | none` | URI scheme used to open citations from Codex output (default: `vscode`). | 配置项说明：参见官方描述。 |
| `otel.environment` | `string` | Environment tag applied to emitted OpenTelemetry events (default: `dev`). | 配置项说明：参见官方描述。 |
| `otel.exporter` | `none | otlp-http | otlp-grpc` | Select the OpenTelemetry exporter and provide any endpoint metadata. | 配置项说明：参见官方描述。 |
| `otel.trace_exporter` | `none | otlp-http | otlp-grpc` | Select the OpenTelemetry trace exporter and provide any endpoint metadata. | 配置项说明：参见官方描述。 |
| `otel.log_user_prompt` | `boolean` | Opt in to exporting raw user prompts with OpenTelemetry logs. | 配置项说明：参见官方描述。 |
| `otel.exporter.<id>.endpoint` | `string` | Exporter endpoint for OTEL logs. | 配置项说明：参见官方描述。 |
| `otel.exporter.<id>.protocol` | `binary | json` | Protocol used by the OTLP/HTTP exporter. | 配置项说明：参见官方描述。 |
| `otel.exporter.<id>.headers` | `map<string,string>` | Static headers included with OTEL exporter requests. | 配置项说明：参见官方描述。 |
| `otel.trace_exporter.<id>.endpoint` | `string` | Trace exporter endpoint for OTEL logs. | 配置项说明：参见官方描述。 |
| `otel.trace_exporter.<id>.protocol` | `binary | json` | Protocol used by the OTLP/HTTP trace exporter. | 配置项说明：参见官方描述。 |
| `otel.trace_exporter.<id>.headers` | `map<string,string>` | Static headers included with OTEL trace exporter requests. | 配置项说明：参见官方描述。 |
| `otel.exporter.<id>.tls.ca-certificate` | `string` | CA certificate path for OTEL exporter TLS. | 配置项说明：参见官方描述。 |
| `otel.exporter.<id>.tls.client-certificate` | `string` | Client certificate path for OTEL exporter TLS. | 配置项说明：参见官方描述。 |
| `otel.exporter.<id>.tls.client-private-key` | `string` | Client private key path for OTEL exporter TLS. | 配置项说明：参见官方描述。 |
| `otel.trace_exporter.<id>.tls.ca-certificate` | `string` | CA certificate path for OTEL trace exporter TLS. | 配置项说明：参见官方描述。 |
| `otel.trace_exporter.<id>.tls.client-certificate` | `string` | Client certificate path for OTEL trace exporter TLS. | 配置项说明：参见官方描述。 |
| `otel.trace_exporter.<id>.tls.client-private-key` | `string` | Client private key path for OTEL trace exporter TLS. | 配置项说明：参见官方描述。 |
| `tui` | `table` | TUI-specific options such as enabling inline desktop notifications. | 终端界面（TUI）相关设置表。 |
| `tui.notifications` | `boolean | array<string>` | Enable TUI notifications; optionally restrict to specific event types. | 是否启用 TUI 通知及其类型。 |
| `tui.notification_method` | `auto | osc9 | bel` | Notification method for unfocused terminal notifications (default: auto). | 未聚焦时的通知方式。 |
| `tui.animations` | `boolean` | Enable terminal animations (welcome screen, shimmer, spinner) (default: true). | 控制终端动画效果。 |
| `tui.alternate_screen` | `auto | always | never` | Control alternate screen usage for the TUI (default: auto; auto skips it in Zellij to preserve scrollback). | 控制 TUI 是否使用备用屏幕。 |
| `tui.show_tooltips` | `boolean` | Show onboarding tooltips in the TUI welcome screen (default: true). | 是否显示新手引导提示。 |
| `hide_agent_reasoning` | `boolean` | Suppress reasoning events in both the TUI and `codex exec` output. | 配置项说明：参见官方描述。 |
| `show_raw_agent_reasoning` | `boolean` | Surface raw reasoning content when the active model emits it. | 展示模型输出的原始推理内容（若可用）。 |
| `disable_paste_burst` | `boolean` | Disable burst-paste detection in the TUI. | 配置项说明：参见官方描述。 |
| `windows_wsl_setup_acknowledged` | `boolean` | Track Windows onboarding acknowledgement (Windows only). | Windows 端 WSL 引导确认状态。 |
| `chatgpt_base_url` | `string` | Override the base URL used during the ChatGPT login flow. | ChatGPT 登录流程使用的基础 URL。 |
| `cli_auth_credentials_store` | `file | keyring | auto` | Control where the CLI stores cached credentials (file-based auth.json vs OS keychain). | CLI 认证凭据的存储位置。 |
| `mcp_oauth_credentials_store` | `auto | file | keyring` | Preferred store for MCP OAuth credentials. | MCP OAuth 凭据的存储方式。 |
| `mcp_oauth_callback_port` | `integer` | Optional fixed port for the local HTTP callback server used during MCP OAuth login. When unset, Codex binds to an ephemeral port chosen by the OS. | 配置项说明：参见官方描述。 |
| `experimental_use_unified_exec_tool` | `boolean` | Legacy name for enabling unified exec; prefer `[features].unified_exec` or `codex --enable unified_exec`. | 实验/旧版兼容选项。 |
| `experimental_use_freeform_apply_patch` | `boolean` | Legacy name for enabling freeform apply_patch; prefer `[features].apply_patch_freeform` or `codex --enable apply_patch_freeform`. | 实验/旧版兼容选项。 |
| `include_apply_patch_tool` | `boolean` | Legacy name for enabling freeform apply_patch; prefer `[features].apply_patch_freeform`. | 配置项说明：参见官方描述。 |
| `tools.web_search` | `boolean` | Deprecated legacy toggle for web search; prefer the top-level `web_search` setting. | 旧版 web_search 开关（优先使用顶层 `web_search`）。 |
| `web_search` | `disabled | cached | live` | Web search mode (default: `\"cached\"`; cached uses an OpenAI-maintained index and does not fetch live pages; if you use `--yolo` or another full access sandbox setting, it defaults to `\"live\"`). Use `\"live\"` to fetch the most recent data from the web, or `\"disabled\"` to remove the tool. | Web 搜索模式（禁用/缓存/实时）。 |
| `projects.<path>.trust_level` | `string` | Mark a project or worktree as trusted or untrusted (`\"trusted\"` \| `\"untrusted\"`). Untrusted projects skip project-scoped `.codex/` layers. | 配置项说明：参见官方描述。 |
| `notice.hide_full_access_warning` | `boolean` | Track acknowledgement of the full access warning prompt. | 配置项说明：参见官方描述。 |
| `notice.hide_world_writable_warning` | `boolean` | Track acknowledgement of the Windows world-writable directories warning. | 配置项说明：参见官方描述。 |
| `notice.hide_rate_limit_model_nudge` | `boolean` | Track opt-out of the rate limit model switch reminder. | 配置项说明：参见官方描述。 |
| `notice.hide_gpt5_1_migration_prompt` | `boolean` | Track acknowledgement of the GPT-5.1 migration prompt. | 配置项说明：参见官方描述。 |
| `notice.hide_gpt-5.1-codex-max_migration_prompt` | `boolean` | Track acknowledgement of the gpt-5.1-codex-max migration prompt. | 配置项说明：参见官方描述。 |
| `notice.model_migrations` | `map<string,string>` | Track acknowledged model migrations as old->new mappings. | 配置项说明：参见官方描述。 |
| `forced_login_method` | `chatgpt | api` | Restrict Codex to a specific authentication method. | 配置项说明：参见官方描述。 |
| `forced_chatgpt_workspace_id` | `string (uuid)` | Limit ChatGPT logins to a specific workspace identifier. | 配置项说明：参见官方描述。 |
