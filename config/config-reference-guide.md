# Codex `config.toml` 配置参考解读

本文基于官方 `config-reference` 页面整理，尽量完整覆盖所有字段，并对关键配置做中文解释。表格中的“官方说明”保留官方英文原文，避免二次翻译造成歧义；下方“重点解读”部分提供中文理解与使用建议。

## 官方来源

- Sample Configuration（示例配置）：https://developers.openai.com/codex/config-sample/
- Config Reference（配置参考）：https://developers.openai.com/codex/config-reference/
- JSON Schema：https://developers.openai.com/codex/config-schema.json

## 编辑器提示（推荐）

在 `config.toml` 顶部加上 schema 行，可以获得自动补全和校验：

```toml
#:schema https://developers.openai.com/codex/config-schema.json
```

## 字段清单（全量）

| Key | Type / Values | 官方说明 |
| --- | --- | --- |
| `model` | `string` | Model to use (e.g., `gpt-5-codex`). |
| `review_model` | `string` | Optional model override used by `/review` (defaults to the current session model). |
| `model_provider` | `string` | Provider id from `model_providers` (default: `openai`). |
| `model_context_window` | `number` | Context window tokens available to the active model. |
| `model_auto_compact_token_limit` | `number` | Token threshold that triggers automatic history compaction (unset uses model defaults). |
| `oss_provider` | `lmstudio | ollama` | Default local provider used when running with `--oss` (defaults to prompting if unset). |
| `approval_policy` | `untrusted | on-failure | on-request | never` | Controls when Codex pauses for approval before executing commands. |
| `sandbox_mode` | `read-only | workspace-write | danger-full-access` | Sandbox policy for filesystem and network access during command execution. |
| `sandbox_workspace_write.writable_roots` | `array<string>` | Additional writable roots when `sandbox_mode = \"workspace-write\"`. |
| `sandbox_workspace_write.network_access` | `boolean` | Allow outbound network access inside the workspace-write sandbox. |
| `sandbox_workspace_write.exclude_tmpdir_env_var` | `boolean` | Exclude `$TMPDIR` from writable roots in workspace-write mode. |
| `sandbox_workspace_write.exclude_slash_tmp` | `boolean` | Exclude `/tmp` from writable roots in workspace-write mode. |
| `notify` | `array<string>` | Command invoked for notifications; receives a JSON payload from Codex. |
| `check_for_update_on_startup` | `boolean` | Check for Codex updates on startup (set to false only when updates are centrally managed). |
| `feedback.enabled` | `boolean` | Enable feedback submission via `/feedback` across Codex surfaces (default: true). |
| `instructions` | `string` | Reserved for future use; prefer `model_instructions_file` or `AGENTS.md`. |
| `developer_instructions` | `string` | Additional developer instructions injected into the session (optional). |
| `compact_prompt` | `string` | Inline override for the history compaction prompt. |
| `model_instructions_file` | `string (path)` | Replacement for built-in instructions instead of `AGENTS.md`. |
| `experimental_compact_prompt_file` | `string (path)` | Load the compaction prompt override from a file (experimental). |
| `skills.config` | `array<object>` | Per-skill enablement overrides stored in config.toml. |
| `skills.config.<index>.path` | `string (path)` | Path to a skill folder containing `SKILL.md`. |
| `skills.config.<index>.enabled` | `boolean` | Enable or disable the referenced skill. |
| `mcp_servers.<id>.command` | `string` | Launcher command for an MCP stdio server. |
| `mcp_servers.<id>.args` | `array<string>` | Arguments passed to the MCP stdio server command. |
| `mcp_servers.<id>.env` | `map<string,string>` | Environment variables forwarded to the MCP stdio server. |
| `mcp_servers.<id>.env_vars` | `array<string>` | Additional environment variables to whitelist for an MCP stdio server. |
| `mcp_servers.<id>.cwd` | `string` | Working directory for the MCP stdio server process. |
| `mcp_servers.<id>.url` | `string` | Endpoint for an MCP streamable HTTP server. |
| `mcp_servers.<id>.bearer_token_env_var` | `string` | Environment variable sourcing the bearer token for an MCP HTTP server. |
| `mcp_servers.<id>.http_headers` | `map<string,string>` | Static HTTP headers included with each MCP HTTP request. |
| `mcp_servers.<id>.env_http_headers` | `map<string,string>` | HTTP headers populated from environment variables for an MCP HTTP server. |
| `mcp_servers.<id>.enabled` | `boolean` | Disable an MCP server without removing its configuration. |
| `mcp_servers.<id>.startup_timeout_sec` | `number` | Override the default 10s startup timeout for an MCP server. |
| `mcp_servers.<id>.startup_timeout_ms` | `number` | Alias for `startup_timeout_sec` in milliseconds. |
| `mcp_servers.<id>.tool_timeout_sec` | `number` | Override the default 60s per-tool timeout for an MCP server. |
| `mcp_servers.<id>.enabled_tools` | `array<string>` | Allow list of tool names exposed by the MCP server. |
| `mcp_servers.<id>.disabled_tools` | `array<string>` | Deny list applied after `enabled_tools` for the MCP server. |
| `features.unified_exec` | `boolean` | Use the unified PTY-backed exec tool (beta). |
| `features.shell_snapshot` | `boolean` | Snapshot shell environment to speed up repeated commands (beta). |
| `features.apply_patch_freeform` | `boolean` | Expose the freeform `apply_patch` tool (experimental). |
| `features.web_search` | `boolean` | Deprecated legacy toggle; prefer the top-level `web_search` setting. |
| `features.web_search_cached` | `boolean` | Deprecated legacy toggle. When `web_search` is unset, true maps to `web_search = \"cached\"`. |
| `features.web_search_request` | `boolean` | Deprecated legacy toggle. When `web_search` is unset, true maps to `web_search = \"live\"`. |
| `features.shell_tool` | `boolean` | Enable the default `shell` tool for running commands (stable; on by default). |
| `features.request_rule` | `boolean` | Enable Smart approvals (`prefix_rule` suggestions on escalation requests; stable; on by default). |
| `features.exec_policy` | `boolean` | Enforce rules checks for `shell`/`unified_exec` (experimental; on by default). |
| `features.experimental_windows_sandbox` | `boolean` | Run the Windows restricted-token sandbox (experimental). |
| `features.elevated_windows_sandbox` | `boolean` | Enable the elevated Windows sandbox pipeline (experimental). |
| `features.remote_compaction` | `boolean` | Enable remote compaction (ChatGPT auth only; experimental; on by default). |
| `features.remote_models` | `boolean` | Refresh remote model list before showing readiness (experimental). |
| `features.powershell_utf8` | `boolean` | Force PowerShell UTF-8 output (defaults to true). |
| `features.child_agents_md` | `boolean` | Append AGENTS.md scope/precedence guidance even when no AGENTS.md is present (experimental). |
| `suppress_unstable_features_warning` | `boolean` | Suppress the warning that appears when under-development feature flags are enabled. |
| `model_providers.<id>.name` | `string` | Display name for a custom model provider. |
| `model_providers.<id>.base_url` | `string` | API base URL for the model provider. |
| `model_providers.<id>.env_key` | `string` | Environment variable supplying the provider API key. |
| `model_providers.<id>.env_key_instructions` | `string` | Optional setup guidance for the provider API key. |
| `model_providers.<id>.experimental_bearer_token` | `string` | Direct bearer token for the provider (discouraged; use `env_key`). |
| `model_providers.<id>.requires_openai_auth` | `boolean` | The provider uses OpenAI authentication (defaults to false). |
| `model_providers.<id>.wire_api` | `chat | responses` | Protocol used by the provider (defaults to `chat` if omitted). |
| `model_providers.<id>.query_params` | `map<string,string>` | Extra query parameters appended to provider requests. |
| `model_providers.<id>.http_headers` | `map<string,string>` | Static HTTP headers added to provider requests. |
| `model_providers.<id>.env_http_headers` | `map<string,string>` | HTTP headers populated from environment variables when present. |
| `model_providers.<id>.request_max_retries` | `number` | Retry count for HTTP requests to the provider (default: 4). |
| `model_providers.<id>.stream_max_retries` | `number` | Retry count for SSE streaming interruptions (default: 5). |
| `model_providers.<id>.stream_idle_timeout_ms` | `number` | Idle timeout for SSE streams in milliseconds (default: 300000). |
| `model_reasoning_effort` | `minimal | low | medium | high | xhigh` | Adjust reasoning effort for supported models (Responses API only; `xhigh` is model-dependent). |
| `model_reasoning_summary` | `auto | concise | detailed | none` | Select reasoning summary detail or disable summaries entirely. |
| `model_verbosity` | `low | medium | high` | Control GPT-5 Responses API verbosity (defaults to `medium`). |
| `model_supports_reasoning_summaries` | `boolean` | Force Codex to send reasoning metadata even for unknown models. |
| `shell_environment_policy.inherit` | `all | core | none` | Baseline environment inheritance when spawning subprocesses. |
| `shell_environment_policy.ignore_default_excludes` | `boolean` | Keep variables containing KEY/SECRET/TOKEN before other filters run. |
| `shell_environment_policy.exclude` | `array<string>` | Glob patterns for removing environment variables after the defaults. |
| `shell_environment_policy.include_only` | `array<string>` | Whitelist of patterns; when set only matching variables are kept. |
| `shell_environment_policy.set` | `map<string,string>` | Explicit environment overrides injected into every subprocess. |
| `shell_environment_policy.experimental_use_profile` | `boolean` | Use the user shell profile when spawning subprocesses. |
| `project_root_markers` | `array<string>` | List of project root marker filenames; used when searching parent directories for the project root. |
| `project_doc_max_bytes` | `number` | Maximum bytes read from `AGENTS.md` when building project instructions. |
| `project_doc_fallback_filenames` | `array<string>` | Additional filenames to try when `AGENTS.md` is missing. |
| `profile` | `string` | Default profile applied at startup (equivalent to `--profile`). |
| `profiles.<name>.*` | `various` | Profile-scoped overrides for any of the supported configuration keys. |
| `profiles.<name>.include_apply_patch_tool` | `boolean` | Legacy name for enabling freeform apply_patch; prefer `[features].apply_patch_freeform`. |
| `profiles.<name>.web_search` | `disabled | cached | live` | Profile-scoped web search mode override (default: `\"cached\"`). |
| `profiles.<name>.experimental_use_unified_exec_tool` | `boolean` | Legacy name for enabling unified exec; prefer `[features].unified_exec`. |
| `profiles.<name>.experimental_use_freeform_apply_patch` | `boolean` | Legacy name for enabling freeform apply_patch; prefer `[features].apply_patch_freeform`. |
| `profiles.<name>.oss_provider` | `lmstudio | ollama` | Profile-scoped OSS provider for `--oss` sessions. |
| `history.persistence` | `save-all | none` | Control whether Codex saves session transcripts to history.jsonl. |
| `tool_output_token_limit` | `number` | Token budget for storing individual tool/function outputs in history. |
| `history.max_bytes` | `number` | If set, caps the history file size in bytes by dropping oldest entries. |
| `file_opener` | `vscode | vscode-insiders | windsurf | cursor | none` | URI scheme used to open citations from Codex output (default: `vscode`). |
| `otel.environment` | `string` | Environment tag applied to emitted OpenTelemetry events (default: `dev`). |
| `otel.exporter` | `none | otlp-http | otlp-grpc` | Select the OpenTelemetry exporter and provide any endpoint metadata. |
| `otel.trace_exporter` | `none | otlp-http | otlp-grpc` | Select the OpenTelemetry trace exporter and provide any endpoint metadata. |
| `otel.log_user_prompt` | `boolean` | Opt in to exporting raw user prompts with OpenTelemetry logs. |
| `otel.exporter.<id>.endpoint` | `string` | Exporter endpoint for OTEL logs. |
| `otel.exporter.<id>.protocol` | `binary | json` | Protocol used by the OTLP/HTTP exporter. |
| `otel.exporter.<id>.headers` | `map<string,string>` | Static headers included with OTEL exporter requests. |
| `otel.trace_exporter.<id>.endpoint` | `string` | Trace exporter endpoint for OTEL logs. |
| `otel.trace_exporter.<id>.protocol` | `binary | json` | Protocol used by the OTLP/HTTP trace exporter. |
| `otel.trace_exporter.<id>.headers` | `map<string,string>` | Static headers included with OTEL trace exporter requests. |
| `otel.exporter.<id>.tls.ca-certificate` | `string` | CA certificate path for OTEL exporter TLS. |
| `otel.exporter.<id>.tls.client-certificate` | `string` | Client certificate path for OTEL exporter TLS. |
| `otel.exporter.<id>.tls.client-private-key` | `string` | Client private key path for OTEL exporter TLS. |
| `otel.trace_exporter.<id>.tls.ca-certificate` | `string` | CA certificate path for OTEL trace exporter TLS. |
| `otel.trace_exporter.<id>.tls.client-certificate` | `string` | Client certificate path for OTEL trace exporter TLS. |
| `otel.trace_exporter.<id>.tls.client-private-key` | `string` | Client private key path for OTEL trace exporter TLS. |
| `tui` | `table` | TUI-specific options such as enabling inline desktop notifications. |
| `tui.notifications` | `boolean | array<string>` | Enable TUI notifications; optionally restrict to specific event types. |
| `tui.notification_method` | `auto | osc9 | bel` | Notification method for unfocused terminal notifications (default: auto). |
| `tui.animations` | `boolean` | Enable terminal animations (welcome screen, shimmer, spinner) (default: true). |
| `tui.alternate_screen` | `auto | always | never` | Control alternate screen usage for the TUI (default: auto; auto skips it in Zellij to preserve scrollback). |
| `tui.show_tooltips` | `boolean` | Show onboarding tooltips in the TUI welcome screen (default: true). |
| `hide_agent_reasoning` | `boolean` | Suppress reasoning events in both the TUI and `codex exec` output. |
| `show_raw_agent_reasoning` | `boolean` | Surface raw reasoning content when the active model emits it. |
| `disable_paste_burst` | `boolean` | Disable burst-paste detection in the TUI. |
| `windows_wsl_setup_acknowledged` | `boolean` | Track Windows onboarding acknowledgement (Windows only). |
| `chatgpt_base_url` | `string` | Override the base URL used during the ChatGPT login flow. |
| `cli_auth_credentials_store` | `file | keyring | auto` | Control where the CLI stores cached credentials (file-based auth.json vs OS keychain). |
| `mcp_oauth_credentials_store` | `auto | file | keyring` | Preferred store for MCP OAuth credentials. |
| `mcp_oauth_callback_port` | `integer` | Optional fixed port for the local HTTP callback server used during MCP OAuth login. When unset, Codex binds to an ephemeral port chosen by the OS. |
| `experimental_use_unified_exec_tool` | `boolean` | Legacy name for enabling unified exec; prefer `[features].unified_exec` or `codex --enable unified_exec`. |
| `experimental_use_freeform_apply_patch` | `boolean` | Legacy name for enabling freeform apply_patch; prefer `[features].apply_patch_freeform` or `codex --enable apply_patch_freeform`. |
| `include_apply_patch_tool` | `boolean` | Legacy name for enabling freeform apply_patch; prefer `[features].apply_patch_freeform`. |
| `tools.web_search` | `boolean` | Deprecated legacy toggle for web search; prefer the top-level `web_search` setting. |
| `web_search` | `disabled | cached | live` | Web search mode (default: `\"cached\"`; cached uses an OpenAI-maintained index and does not fetch live pages; if you use `--yolo` or another full access sandbox setting, it defaults to `\"live\"`). Use `\"live\"` to fetch the most recent data from the web, or `\"disabled\"` to remove the tool. |
| `projects.<path>.trust_level` | `string` | Mark a project or worktree as trusted or untrusted (`\"trusted\"` \| `\"untrusted\"`). Untrusted projects skip project-scoped `.codex/` layers. |
| `notice.hide_full_access_warning` | `boolean` | Track acknowledgement of the full access warning prompt. |
| `notice.hide_world_writable_warning` | `boolean` | Track acknowledgement of the Windows world-writable directories warning. |
| `notice.hide_rate_limit_model_nudge` | `boolean` | Track opt-out of the rate limit model switch reminder. |
| `notice.hide_gpt5_1_migration_prompt` | `boolean` | Track acknowledgement of the GPT-5.1 migration prompt. |
| `notice.hide_gpt-5.1-codex-max_migration_prompt` | `boolean` | Track acknowledgement of the gpt-5.1-codex-max migration prompt. |
| `notice.model_migrations` | `map<string,string>` | Track acknowledged model migrations as old->new mappings. |
| `forced_login_method` | `chatgpt | api` | Restrict Codex to a specific authentication method. |
| `forced_chatgpt_workspace_id` | `string (uuid)` | Limit ChatGPT logins to a specific workspace identifier. |


## 重点解读（中文）

### 模型相关

- `model`、`review_model`、`model_provider`：决定默认使用的模型与供应商。`review_model` 只影响 `/review` 场景。
- `model_context_window`、`model_auto_compact_token_limit`：控制上下文窗口与自动压缩触发阈值，影响长对话的稳定性。
- `model_reasoning_effort`、`model_reasoning_summary`、`model_verbosity`：影响推理强度、推理摘要与回答冗长度（在支持的模型/协议下生效）。

### 执行安全与审批

- `approval_policy`：控制在执行命令前是否需要批准，适合团队或生产环境提高安全性。
- `sandbox_mode` 与 `sandbox_workspace_write.*`：控制命令执行时的文件系统与网络权限，影响能否写入工作区或访问外网。

### 指令与压缩

- `developer_instructions`、`model_instructions_file`：用来注入额外开发者指令或替换默认指令。
- `compact_prompt` 与 `experimental_compact_prompt_file`：自定义历史压缩提示词，适合对上下文压缩效果有更高要求的场景。

### MCP 服务器

- `mcp_servers.<id>.*`：配置 MCP 服务器（stdio 或 HTTP），可扩展工具能力。常见要素包含启动命令、参数、环境变量、超时和工具白/黑名单。

### Skills

- `skills.config` 与子项：用于在 `config.toml` 中启用/禁用指定技能，适合多技能并存的场景。

### 功能开关

- `features.*`：包含实验功能与兼容开关，通常只在明确需要时启用。
- `suppress_unstable_features_warning`：用于抑制实验特性警告。

### 终端体验（TUI）

- `tui.*`：控制终端 UI 的动画、通知方式、提示信息等体验细节。

### 网络搜索

- `web_search` 与 `tools.web_search`：控制 Web 搜索工具是否可用及模式（`disabled` / `cached` / `live`）。优先使用顶层 `web_search`。
