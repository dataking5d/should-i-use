# Output shapes and Decision Record

Three separate things, which must not be confused:

| | Audience | When |
|---|---|---|
| **Decision Brief** (§1) | the user, in chat | **default** |
| **Full report** (§2) | the user, in chat | escalation only |
| **Decision Record** (§3) | a future re-evaluation | Standard/Deep |

**Internal record complete ≠ reply complete.** The record keeps everything worth re-reading later.
The reply shows only what is needed to make *this* decision now. Never dump record fields into the
reply to justify the record.

---

## The evidence rule — decision-critical only

Before an item enters the reply it must answer: **which judgement does it change?** If the answer
is "it helps understand the project", it is supporting material and belongs in the record.

- **Supporting by default** (record only, unless it bears on the verdict): stars, forks, commit
  counts, contributor counts, creation date, general repo metadata, feature lists, architecture
  descriptions that do not feed the increment comparison.
- **Decision-critical by default**: whether the capability is already covered, by what, and whether
  it is reusable *there*; the mechanism of the real increment; the specific defect or
  incompatibility that rules adoption in or out; the cost that cancels the increment; the cheaper
  path; the Demand Confidence behind any "no need" premise; licence, privacy or permission
  constraints that actually touch the user.

A grade is not a substitute for relevance. `[事实-已验证]` star counts are still supporting
evidence.

---

## §1 — the Decision Brief (default)

**Not rendered while a decision-critical unknown is unresolved** — a Question-only response replaces
the whole Brief (SKILL.md, Decision-Critical Output Gate). The one exception is a decision-critical
unknown that *cannot* be settled cheaply: then the limit belongs inside the conclusion itself.

Chinese output. Seven slots, this order. **The causal chain is written exactly once — in slot 2.**
Slots 4 and 7 state the action; they do not re-argue it.

### 1. 决策
1–3 sentences: the action, then the single most important reason.
> 不建议引入：三项头牌机制分别已被 A、B、C 覆盖，唯一独有的部分在本机开箱即坏。

### 2. 为什么
3–5 bullets — **only the factors that decided it.** Not a summary of the investigation, not every
finding that was made. This is the one place the causal chain appears.

### 3. 增量对照
Only the capabilities that mattered, in four columns:

| 它提供 | 我已有 | 真正新增 | 获取成本 |
|---|---|---|---|

The **我已有** cell states *where* the capability lives and whether it is reusable **there**
(SKILL.md rule 5). "某项目里用过" is not coverage; a localised capability is not an available one.
Any generalisation cost found this way belongs in **真正新增** or **获取成本**, not smoothed over.

Capabilities that were never in question are left out.

### 4. 整体 / 局部
Two lines by default:
```
整体：不采用 — <一句原因>
局部：无 — <或  局部：吸收 X — 一句原因>
```
Expand only when the split itself is non-obvious and the reasoning needs it.

### 5. 未知项
Only unknowns that **could change the action**, each with what would resolve it. An unknown that
cannot change the action is not listed. If there are none, omit the section. A decision-critical
unknown that *can* be settled by one cheap question does **not** belong here — the whole Brief is
withheld and that question is the response (SKILL.md, Decision-Critical Output Gate). Only one that
**cannot** be settled cheaply appears here, and it must also weaken slot 1's verdict rather than sit
beside an unqualified action.

### 6. 顺带发现
Optional. At most 2–4 bullets, each: what was found · why it might matter · **whether it affects
the current decision**. If it does not, say so in one clause — *与当前采用判断无关，建议另开任务处理* —
and stop.

### 7. Action
One line. The action, not a second argument.
> Action：无需行动
> Action：进入隔离试验 — 只读跑 X，不安装、不改配置
> Action：另开任务处理花名册引用问题（与本项目判断无关）

### When to escalate to the full report

- the user asks for it explicitly
- the decision is major — long-lived infrastructure, a stable main chain, money or privacy
- two options are genuinely close
- the evidence conflicts
- the verdict rests on a chain of facts a brief cannot carry

Format preference is not a reason. "The user might want detail" is not a reason.

---

## §2 — the full report (escalation)

Same content, expanded. Headings are a spine: keep the order, drop what carries nothing, and name
what you dropped. The evidence rule still applies — length is not a licence to pad, and a request
for the full report is not a licence to skip the Output Gate either.

### 1. 一句话结论
The single most important judgement, stated plainly. Not hedged into meaninglessness.

### 2. 它到底是什么
What problem it actually solves, in plain technical language. Not a README restatement — if the
project's own framing is misleading, say what it really is.

### 3. 对我的真实增量价值
The four columns from workflow step 4, expanded: **我目前有什么 · 它新增什么 · 哪些是重复能力 ·
哪些是真正的新能力.** Name the mechanisms, not the feature names.

### 4. 关键证据
Decision-critical evidence only, each graded and sourced. No padding.

### 5. 接入成本与风险
Only the costs that bear on this decision. End by answering: do these costs cancel the increment?

### 6. Whole-project Value
两到三行，除非推理确实复杂。

### 7. Component Value
What is worth 拆 / 学 / 移植 / 重写 / 借鉴. If nothing, write **无** — do not pad with faint praise.

### 8. 替代方案
Only when weeding is genuinely needed: an obvious defect, a signal that something more mature
exists, a weak match, or an alternative that would clearly cut integration cost. A short comparison,
never a "Top 10" list.

### 9. 顺带发现
As brief slot 6. ≤4 bullets. Never a second audit.

### 10. 当前决策
One verdict from the vocabulary in `SKILL.md`. The causal chain was written in §2 — reference it,
do not restate it.

### 11. Action
One line. On **Deep** depth, precede it with **改口条件** — the 1–3 conditions that would flip the
verdict (§3 explains what qualifies).

---

## §3 — the Decision Record

Save to `records/<project-slug>.md`. One file per project. `<project-slug>` = lowercase repo or
project name, hyphens for spaces. Re-analyzing an existing project: update that file, add the new
date under a `## 复查` heading rather than creating a second file.

The record is where completeness is allowed — it is written for a future reader who has forgotten
everything. Completeness there never obliges the reply to be complete too.

```markdown
---
project: <name>
url: <address>
type: <skill | mcp | framework | db | memory-rag | lib | app | model | prompt | paper | other>
analyzed: YYYY-MM-DD
verdict: <整体判断 — one of the vocabulary terms>
components: <局部判断 — 部分吸收 | 无>
record_status: <active | superseded>
---

# <project>

**核心用途** — one sentence.

**与现有系统的关系** — overlap, complement, or genuinely additive. Name the specific thing of
mine it overlaps with, or state that nothing overlaps.

**Whole-project decision** — verdict + one-line cause.

**Valuable components** — what is worth absorbing even if the whole is rejected. Or: 无.

**核心理由** — the causal chain, 2-4 lines. Why this verdict and not the neighbouring one.

**主要风险** — the costs that would actually bite.

**未验证事项** — what remains `[未知]`, and what would resolve it. The queue for a future isolated
trial.

**改口条件** — the 1–3 conditions that would genuinely flip this verdict, so a later re-read knows
when to reopen it. Only real ones: a change in the user's own situation (frequency of use, a new
need), a new capability in the project (a CJK-aware branch, a fixed defect), or an observed failure
that creates the need. Omit the field when the verdict is robust — never generate conditions to
fill it.

**下一步** — the smallest next step, or 无需行动。
```

Then append one line to `records/_index.md`:

```markdown
| YYYY-MM-DD | [name](name.md) | type | verdict | one-line hook |
```

### Why plain files
One markdown file per project keeps this readable now and trivially migratable later — into a
knowledge base, a database, or an Obsidian vault — without a schema rewrite. Do not build a
database for this. If the record count ever makes the flat index painful, that is the moment to
index it, not before.
