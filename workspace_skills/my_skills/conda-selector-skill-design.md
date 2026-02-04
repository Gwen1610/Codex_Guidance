# Conda Selector（Skill 设计草案，简化版）

目标：这是一个**只管“用哪个 Python”**的简单选择器。

## 行为规则（你要求的最小逻辑）

1) 默认一律使用 conda 环境 `yellow`
2) **除非你显式要求切换**（例如“切到 blue/white/cherry”），否则**永远不切换**
3) 不做任何“根据用途自动匹配环境”的推断
4) 如果 `yellow` 在环境层面不可用（见下），我会**询问你要不要切换环境**（例如切到 `blue/white/cherry/base` 或直接用系统 `python3`）

## 什么叫“yellow 在环境层面不可用”

只在这些情况触发“询问是否切换环境”：

- `conda` 命令不可用（PATH 找不到 conda）
- `conda run -n yellow python ...` 无法启动解释器（比如 env 不存在 / env 没有 python / conda run 自身报错导致 python 没启动）
- 或其他**导致 `conda run -n yellow python ...` 在启动阶段就失败、解释器未能启动**的情况（仅限启动阶段错误）

注意：**脚本运行过程中的报错**（例如缺包、代码异常、断言失败等）不触发自动切换；仍按“默认 yellow”继续，除非你主动说要切换环境。

## 交互方式（建议用自然语言）

- “这次用 blue”
- “切回 yellow”
- “别用 conda，改用系统 python3”

## 会话内切换规则

当你说“这次用 blue”，按你的决定：**当前会话一直用 blue，直到你说切回 yellow**。
