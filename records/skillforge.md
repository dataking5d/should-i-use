---
project: SkillForge
url: https://github.com/tripleyak/SkillForge
type: skill
analyzed: 2026-09-24
verdict: 没必要引入
components: 无
record_status: active
---

# SkillForge

**核心用途** — 一个「技能工厂 / 技能生态维护」元 skill：以基线失败为前提生成 skill，产出带 `evals/` 回归套件的技能，并提供跨 skill 生态体检（触发词冲突、失效引用、token 预算）与 opt-in 的主动推荐 advisor。

**与现有系统的关系** — 高度重叠，四条线各自撞上已有能力：

| 它的机制 | 我已有的对应物 |
|---|---|
| RED/GREEN 基线门（先看 agent 失败才写 skill） | superpowers `writing-skills` 的核心原则就是这句话，且带 `testing-skills-with-subagents.md` |
| `evals/` + `run_skill_evals.py --live` | 官方插件 `skill-creator` 已含 evals / benchmark / 方差分析 / eval-viewer |
| `compile_skill.py --target codex\|agentskills` + 64/1024 字段规范 | `agent-skill-creator` v4.0.0 已含 64/1024 限制、`references/cross-platform-guide.md`、`install.sh --platform codex` |
| 生态 doctor（6 项检查） | 无对应物，但 6 项中 4 项是一行命令级别的检查，唯一非平凡的一项（触发词冲突）对本机数据失效 |

**Whole-project decision** — 没必要引入。头牌机制（基线门、回归 evals、跨运行时编译）三项全部已被现有三个工具覆盖；唯一独有的 doctor 用不上又算得出来；advisor 在本机开箱即坏。

**Valuable components** — 无。

**核心理由** —
1. 真正独有的只剩 doctor + advisor。advisor 的钩子命令由 `hook_command()` 硬编码为 `python3 <path>`（`install_skillforge.py:99`），而本机 `python3` 是 Microsoft Store 的 App Execution Alias 空壳，跑起来只会打印 "Python was not found" —— 它想装成「每条 prompt 都跑的」首个全局钩子，开箱即坏。
2. doctor 的 6 项检查里，(b)(e)(f) 是一行命令级；唯一非平凡的 (a) 触发词冲突走 `content_words()`（`_WORD_RE = [a-z0-9]+`，ASCII only），我 32 个 skill 中 15 个描述为中文，该检查对它们近乎盲区——同一份代码里也不存在任何 CJK 处理。
3. 剩下的检查虽有真增量（本次实测确实查出 4 处失效引用），但有更便宜的路径：一个 20 行 `Path.exists()` 循环即可，无需引入 11.5k 行 Python。
4. 项目本身工程质量不差（consent 门、敏感信息 redact、settings.json 先备份再合并、拒绝覆盖畸形配置、自带外部审计报告），但质量不是适配。

**主要风险** —
- 首个全局钩子会在每条 prompt 上起一个 Python 进程，并向上下文注入一行指令文本；`python3` 前缀在本机必失败。
- 安装动作要改 `~/.claude/settings.json`（敏感文件，此前有过 key 泄漏史）。
- `mine_skill_friction.py` 会读 `~/.claude/projects/*/*.jsonl`（本机即 D 盘全部会话记录）；隐私设计克制（--consent 硬门 + redact），但 `is_correction()` 的 `CORRECTION_PHRASES` 全是英文，中文会话的「纠正/放弃」信号会被整体漏掉。

**未验证事项** —
- 未执行任何 SkillForge 代码（只读分析）。「中文 prompt 走 triage 得 0 分」是代码推断，未实跑验证。
- `validate_skill.py` / `run_skill_evals.py --live` 在我实际 skill 上的真实表现未测。
- 我的失效引用扫描是自己的正则，对「省略目录前缀的简写引用」会误报（`guizang-ppt-skill` 的 10 处误报已人工排除）；lark-* 的同类误报未逐条排除。

**改口条件** — ① 我开始高频写 skill（每周+）；② SkillForge 出现 CJK 感知的索引/doctor 分支；③ 某个自建 skill 悄悄跑偏过，那才值得为 GREEN 门/evals 接线。三条都不是时间触发。

**下一步** — 无需行动。不必安装、不必隔离试验。真正值得做的与 SkillForge 无关：跑一次自己 skill 花名册的引用体检（见报告 Opportunity Discovery）。
