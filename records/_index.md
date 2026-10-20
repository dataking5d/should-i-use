# Decision Records

One file per analyzed project. Written by the `should-i-use` skill.

| 日期 | 项目 | 类型 | 决策 | 一句话 |
|---|---|---|---|---|
| 2026-09-24 | [skillforge](skillforge.md) | skill | 没必要引入 | 基线门/evals/跨运行时编译三项分别撞上 writing-skills、skill-creator、agent-skill-creator；仅剩的 advisor 因 `python3` 在本机开箱即坏，doctor 的触发词冲突检查对中文描述是盲区 |
| 2026-09-24 | [serena](serena.md) | mcp | 暂不采用 | 检索半边已被 codegraph 覆盖，只剩符号级写操作；Windows 税与永久上下文税超过增量 |
| 2026-09-24 | [mem0](mem0.md) | memory-rag | 没必要引入 | 自动抽取+语义召回与 ACP 反向，成本落在真实处、增量落空 |

Newest first.
