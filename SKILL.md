---
name: should-i-use
description: >-
  Use when the user shares an open-source repository, package, MCP server, agent skill,
  framework, database, memory/RAG system, CLI, SDK, or technical article and asks whether it is
  worth adopting — e.g. "分析这个项目 <URL>", "看看这个 MCP 值不值得我接", "要不要引入 X",
  "这个 skill 对我有用吗", "should I use this", "is this worth installing", "帮我做个技术尽调",
  "和我现有方案对比一下", "这个东西值不值得拆". Also use when re-evaluating a previously analyzed
  project, or listing past adoption decisions. Read-only due diligence: never installs, executes,
  or reconfigures anything.
---

# should-i-use

Decide whether an external technical resource **earns a place in the user's system**. Not whether
it is a good project.

## Layers — what this skill requires, and what merely helps it

| Layer | What it is | Required? |
|---|---|---|
| **0. Agent baseline** | Web access, file reading, reasoning, tool use. Nothing to teach. | assumed |
| **1. This skill** | The methodology below. **Self-contained: assumes no decision profile, no ecosystem map, no decision history, no host-specific global rules.** | **required** |
| **2. Environment overlay** | A `profile.md`, an existing `records/` history, and whatever rules the host environment already enforces. When present, it sharpens the analysis and saves work. | **optional** |

**Design contract: Layer 2 may enhance Layer 1. Layer 1 must never depend on it.**

If `profile.md` is absent, `records/` is empty, and the host enforces no global rules at all, this
skill must still produce a complete, *user-specific* adoption decision. A version of this skill
that silently degrades into generic project evaluation when the overlay is missing is broken.
Every rule below is written to hold on its own.

## The stance

"好项目" and "对这个用户值得引入的项目" are different claims. A well-built, popular, technically
impressive project can still be the wrong move: the user may already have the capability, the
increment may be small, the complexity or maintenance cost may exceed the benefit, or there may be
a cheaper way to get the same thing.

Three outcomes are equally valid, and the third is frequently correct:

- 整体采用 / 改造后采用
- 局部吸收 — take only a component, schema, algorithm, routing strategy, or prompt
- **什么都不做** — never manufacture a recommendation to look useful

Stars, hype and README adjectives never drive the verdict. Neither does a score.

## Rule 0 — the sufficiency gate

Run this before **every** additional tool call:

> **Is the evidence already sufficient to decide 采用 / 不采用 / 需要进一步验证?**

If it is, **stop investigating.** Making the report more complete is not a reason to keep going.

The decision is already determined when any one of these holds:

- the core capability is already covered by what the user has
- the residual increment is thin
- an obviously cheaper path to the same increment exists
- one decisive compatibility problem rules adoption out
- the cost plainly exceeds the residual increment
- every remaining unknown, taken at its most favourable, would not change the action

Keep going only for an unknown with a realistic chance of **flipping the verdict**. Investigation
completeness is not a goal. **Enough evidence for a correct decision is the goal.** Which unknowns
may be skipped is therefore two classes, not one:

- **Ordinary unknown** — answered even in the project's favour, it would not change the action.
  Rule 0 skips it (it is the last determination condition above).
- **Decision-critical unknown** — a *different* answer would realistically flip the action
  (无需行动 → 隔离试验). **Rule 0 may never skip this class.**

So before every stop, ask: *of what is still unconfirmed, can any answer flip the action?* If one
can, and **one** cheap, minimal question to the user settles it, ask that question **before** giving
the verdict — the smallest question that could change the action, never a background questionnaire.
If it cannot be settled cheaply, **do not quietly assume an answer**: lower the strength of the
conclusion instead — `当前无需引入`, `暂不采用`, or `需要确认 X 后再决定是否试验`.

**Only an answer that can flip the action justifies interrupting the user.** Doubt alone does not —
this is not "when in doubt, ask".

Three corollaries:

1. A question that would only make the report read better is not investigated.
2. Once the verdict is settled, a problem noticed elsewhere is a 旁路 finding (workflow step 9),
   not a new investigation.
3. **A project that will not be adopted does not get engineering effort.** If a defect could only
   be fixed by forking, and adoption is already rejected, the defect is a footnote in the risk
   list — not a work item. Investigate a fix only when the *difficulty* of fixing it is itself what
   decides adoption.

## Core rules (self-contained)

1. **Read-only.** Inspect, analyze, compare, recommend. Never install, execute project code, start a
   service, register an MCP, edit configs, skills, agents, or projects. Cloning into a temp dir for
   static reading is fine; running it is not.
2. **Verify load-bearing facts.** Do not assert from memory. Look it up in docs, source, issues,
   releases, changelog. If you cannot check, write `[未知]`.
3. **Grade every load-bearing claim.** Marketing language is a claim, not a fact.
4. **Project quality ≠ fit.** Assess the increment *for this user*, never in the abstract.
5. **Verify what the user already has** rather than assuming — and judge *availability*, not
   presence. Unverified recollection is not evidence of overlap, and neither is "this technology
   appeared somewhere in one of their projects". Only the first row below is coverage:

   | Finding | Verdict |
   |---|---|
   | Exists as a general, callable capability | covered |
   | Exists, but serves one project only | **not** covered — add the generalisation cost |
   | Exists as a low-level primitive with no shared wrapper | **not** covered — the wrapper is the cost |
   | Migratable, but migration itself is work | **not** covered — migration cost is real cost |
   | Functionally similar, different execution form (agent-driven vs callable, one-off vs batch) | **not** equivalent |

   The test is **can the user actually use this capability, at reasonable cost, in the place that
   needs it** — not whether the technology has ever appeared on their machine. When a capability
   exists but is localised, its generalisation cost is part of the real increment and the
   migration cost, and must be priced there.
6. **Analyze real increment**, not feature lists. For each claimed capability: what does the user
   already have, what is genuinely new, what is merely a different implementation?
7. **Separate whole-project value from component value.** Two independent verdicts, both required.
8. **Weigh cost against increment.** Not "is this a risk" but "does this cost cancel the increment".
9. **Check for a cheaper path to the same increment** immediately after the increment is
   established, *before* costing the full adoption. A genuine cheaper path is decisive and sharply
   lowers whole-project value. Compare against what the user already has and against the obvious
   primitive (a CLI, a library, a loop) — never against the whole market.
10. **"无需行动" is a legitimate verdict.** So is "需要更多证据".
11. **Stars, forks, hype and README polish never enter the reasoning.** Context at most, never cause.
12. **Analysis and implementation are separate phases.** If a verdict-critical question can only be
    settled by running something, stop, say so, propose an isolated trial, and wait for authorization.
13. **The reply carries decision-critical evidence only.** Every item shown must answer *which
    judgement it changes*. Descriptive material — stars, forks, commit totals, general repo
    metadata — belongs in the Decision Record. **Internal record complete ≠ reply complete.**
14. **"No demand found" is not "no demand."** A scan of the registry, the current project, greps
    over the user's own code, and `records/` establishes what has been *observed*, not what
    exists. Unless the user has said they do not need it, a design decision excludes it, or the
    project boundary proves it absent, the finding is worded: *the currently observed ecosystem
    shows no demand sufficient to justify adopting this* — never *you don't need this*. See
    **Demand Confidence** under the judgement model.

## Evidence grades

| Grade | Meaning |
|---|---|
| `[事实-已验证]` | Confirmed against a source you actually read — cite it |
| `[代码证据]` | Confirmed by reading source, config, schema or tests |
| `[项目方声明]` | README / site / author claim, not independently verified |
| `[推断]` | Technical inference — state the premise it depends on |
| `[未知]` | Evidence insufficient. Say so; never fill the gap by guessing |

*10x faster · production ready · state of the art · zero config · enterprise grade · blazing fast*
are `[项目方声明]` until you find the benchmark, code path or test that demonstrates them.

**Absence from the README is not evidence of absence.** Look in official docs → source → issues →
releases → discussions → changelog before writing `[未知]`. Conversely, **absence from your search is
not evidence of absence** — confirm your search scope was reasonable before concluding something
does not exist.

## Workflow

**1. Classify, and size the investigation.** Identify the resource type — it selects the
investigation focus and which cost axes matter (see `references/playbooks.md`). Never apply one
fixed checklist to everything.

Size the investigation on **three** factors, not one:

| Factor | Question |
|---|---|
| **潜在影响** | If adopted, how much would it touch — a stable main chain, a global config, a security/privacy boundary, long-lived infrastructure? |
| **决策剩余不确定性** | How much of the verdict is still genuinely open *after the cheap checks*? |
| **获取额外证据的价值** | Would the next investigation step plausibly change the action? |

Depth is the product of all three:

| Depth | When | What it means |
|---|---|---|
| **Quick** | Reversible and low-impact — **or** high-impact that cheap static analysis has already resolved | Four-column increment check + cheaper-path check + cost-vs-increment + one-line verdict. No Decision Record. |
| **Standard** | The default: real but bounded impact, and residual uncertainty a normal investigation resolves | Decision Brief + Decision Record. |
| **Deep** | Long-lived infrastructure, a stable main chain, a security/privacy boundary, or a decision the user has revisited — **and** residual uncertainty that actually survives the cheap checks | Standard + alternatives and the 1–3 conditions that would change your mind. (Adversarial scrutiny of the verdict is required at *every* depth where impact is high — see below.) |

**Impact alone does not mandate Deep.** A project that *would* touch a global config is not
automatically Deep: if static analysis already shows the core capability is covered, the residual
increment thin and a cheaper path available, the decision is settled and the investigation stops
there. Conversely a small, harmless-looking skill whose boundaries against several existing
skills or MCPs are tangled may need Standard. Size to the **residual uncertainty**, not to the
worst case imaginable.

**But high impact always mandates adversarial scrutiny of the verdict.** Stopping the *evidence
gathering* early is not the same as skipping scrutiny of the conclusion. When impact is high and
uncertainty is low — a project that would touch a global config but whose increment is already
covered — stop investigating, then state the strongest argument *against* your own verdict and
answer it before writing the verdict down. That check costs reasoning, not tool calls, and it is
what makes an early stop safe rather than lazy.

**2. Establish the decision context.** The verdict is *for this user*, so their situation is
load-bearing, not decoration.

- If the environment provides a profile or ecosystem map (Layer 2), use it.
- If it does not, **derive what you can from the environment you are actually in**: the working
  directory and its manifests (`package.json`, `pyproject.toml`, `requirements.txt`, lockfiles),
  nearby projects, git history, config and dependency files, registered tools.
- Only then, if a still-missing fact would **materially change the verdict**, ask the minimum
  question set — at most three, each one whose answer could flip the decision. The usual
  load-bearing unknowns: what the user does *instead* today (overlap), the environment it must run
  in, whether this is one-off or long-term infrastructure, and which cost they least want to pay.
- **Never degrade into a generic project review.** "Well-built with an active community" is not a
  verdict. If you cannot get enough context to be user-specific, state exactly what is missing and
  which verdicts you can and cannot support — do not substitute a generic scorecard.

**3. Investigate.** Depth follows the decision, not a ritual — and Rule 0 runs continuously. The
README is a starting point, never the source. Follow the type's playbook into source, config,
dependencies, releases, issues, changelog, tests, licence, deployment and security model **as far
as the decision requires, and no further**.

**4. Establish the user's baseline.** For every capability the project claims, check what the user
already has — and judge its **availability**, not its presence (rule 5). Produce four columns:
**外部项目能力 / 用户已有 / 真实新增 / 迁移整合成本**. The 用户已有 cell states *where* the capability
lives and whether it can actually be reused at that place; a generalisation cost discovered here
belongs in the last two columns, not hidden. State plainly when the real answer is "只是换一种实现
方式". Stop once every capability the verdict depends on has been checked — this is an inventory
read, not an audit of the user's system.

**5. Cheaper-path check — immediately, before costing the full adoption.** Ask in order:

1. Is this increment one the user actually wants?
2. Can a tool they already have produce it?
3. Can a small script, CLI call, or config change produce it?
4. Does getting it require taking on the whole project, or only reading it?

A genuine cheaper path is a decisive reason not to adopt, and is frequently the real answer. When
one exists, whole-project value drops sharply — say so plainly and do not go on costing the full
adoption. **Do not turn this into a competitor search:** "something better might exist" is not a
finding.

**6. Weigh cost against increment.** The six baseline checks plus the type-specific axes from the
playbook — but only those that bear on this decision. Answer explicitly: does this cost cancel the
increment?

**7. Split whole vs component.** Two independent verdicts. A project can be 整体不采用 and still
contain a schema, routing strategy, prompt, algorithm or memory design worth absorbing.

**8. Decide, then record** — unless the Output Gate is in its question-only state: if an unresolved
decision-critical unknown can be settled by one cheap question, that question *is* the entire
response and nothing is decided or recorded yet. Otherwise: verdict plus the causal chain that
produced it. On high-impact decisions, put the strongest counter-argument to your own verdict first
and answer it — an early stop is only safe if the conclusion survived that. Write the Decision
Record and append one line to `records/_index.md` (templates in `references/report.md`). Skip the
record on Quick depth. The record may be more complete than the reply — that is the point of it.

**9. Side findings — 旁路输出.** A gap in the user's own system the investigation happened to
expose. Keep it strictly separate from the adopt verdict and never let it take over the report: at
most 2–4 bullets, each saying what was found, why it might matter, and **whether it affects the
current decision**. If it does not, write *与当前采用判断无关，建议另开任务处理* and stop. **Never expand a
side finding into a second investigation here** — no ecosystem audit, no per-item verification of
the new problem's scope.

## Judgement model

The verdict follows from causes, not from a tally:

- 它解决什么问题？
- 有证据表明这个用户有这个问题吗？（注意证据强度，见下）
- 是不是已经解决了？已有到什么程度、能不能复用？（rule 5）
- 它真正新增了什么？
- 为了这个新增要付出什么？
- 有没有更低成本的方式拿到同样的东西？

### Demand Confidence — how much weight the "no need" premise can carry

Whenever a verdict leans on "the user has no need for this", say how strong that premise is:

| Level | Meaning |
|---|---|
| **High** | The user said so, a design decision excludes it, or a project boundary proves it absent |
| **Medium** | Several real projects or records support it |
| **Low** | Only that the current scan did not find it |

The scan covers the project registry, the current project, greps over the user's own code,
`records/`, and known workflows. It does **not** cover unregistered experiments, abandoned work, or
anything the user has not written down.

If a "don't adopt" verdict depends heavily on a **Low**-confidence demand finding, treat "no need"
as `[推断]`, not fact — and do one of:

- soften the verdict to a present-tense **当前无需引入** rather than an absolute 没必要引入;
- ask the user **one** minimal question when the answer could genuinely flip the decision — that
  makes the demand premise a decision-critical unknown, and Rule 0 governs it;
- expose the premise explicitly as `[推断]` in the reply.

**Never upgrade "not found" into "does not exist".**

Verdict vocabulary — pick what the evidence supports and explain the causal chain:

`直接采用` · `隔离试验` · `改造后采用` · `部分吸收` · `保持观察` · `暂不采用` · `没必要引入` · `需要更多证据`

## Output — Decision Brief by default

**Decision-Critical Output Gate — this outranks the Brief contract below.** While a
decision-critical unknown (Rule 0) is unresolved, the normal Decision Brief is not a permitted
output. Two states:

- **One cheap question resolves it → a Question-only response.** One sentence of why it must be
  confirmed, then the single minimal question, then **stop.** None of these may appear: a verdict;
  `暂不采用` / `没必要引入` / `值得采用` / `隔离试验`; Whole-project or Component Value; Action; the Brief;
  "当前决定是…"; or a conclusion followed by "不过我还需要问…". A provisional direction may exist
  internally; it may not leave as user-visible output.
- **Not cheaply resolvable → a provisional conclusion with the limit inside it.** The limitation is
  part of the conclusion, never parked in 未知项 or a footnote:

  > 当前证据只能支持"暂不引入"；如果 X 为真，行动会改变为隔离试验。

With no unresolved decision-critical unknown: report in the user's language. The default deliverable
is a **Decision Brief** (7 slots, full contract in `references/report.md`), not a long report:

**决策** → **为什么** → **增量对照** → **整体 / 局部** → **未知项** → **顺带发现** → **Action**

- **决策** is 1–3 sentences: the action, then the single most important reason.
- **为什么** carries only the 3–5 factors that actually decided it. The causal chain is written
  **once**, here.
- **整体 / 局部** are one line each by default.
- **未知项** lists only unknowns that could change the action. If none can, omit the section.
- The full report is an **escalation**, used only when the user asks for it, the decision is major
  (long-lived infrastructure, a stable main chain, money or privacy), two options are genuinely
  close, the evidence conflicts, or the verdict rests on a chain of facts a brief cannot carry.
  Format preference is not a reason to escalate.

Lead with the decision, never with background. If the verdict is 不用, the Action may legitimately
be **无需行动。**

## Red flags — you have drifted

**Into a generic project reviewer:**

- The verdict would read the same for any user; their existing stack never appears
- "它有 X 功能，所以值得安装"
- Stars, forks or hype appear in the reasoning that decides adoption
- A score, grade or percentage is doing the deciding
- Every section is filled, but no causal chain connects evidence → verdict
- `[项目方声明]` quoted as established fact
- "用户大概已经有类似的" — an unverified recollection asserted as fact
- Whole-project and component verdicts collapsed into a single answer
- Adoption recommended without checking for a cheaper path to the same increment
- A recommendation appears because the report felt empty without one
- A technology that appeared somewhere in the user's projects is written up as a reusable
  capability ("they already have X") without checking whether it can be used where it is needed
- "Not found in my scan" written as "the user does not need this"

**Into over-investigation — the opposite failure:**

- Investigation continued after the verdict was already determined
- A tool call was made for a fact that changes no judgement
- A question was put to the user whose answer could not change the action
- An unresolved decision-critical unknown existed, and the response still carried a verdict, an
  Action, or a normal Decision Brief before asking the resolving question
- Stars, forks, commit counts or repo metadata occupy space in the reply
- The same conclusion is restated in three sections
- A side finding grew into a second investigation inside this report
- Engineering effort spent on fixing a project that is not being adopted

**Any of these means: stop. Return to step 4 and re-ask Rule 0.**

## Files

| File | Required? | Purpose |
|---|---|---|
| `profile.md` | **optional** | Environment overlay: decision profile + ecosystem map. Absent by default; the skill works without it. |
| `references/playbooks.md` | required | Per-type investigation focus and cost axes. Extend when a new type appears. |
| `references/report.md` | required | Decision Brief contract, full-report escalation, Decision Record template. |
| `references/clean-room-test.md` | maintainer-only | Validation protocol for measuring standalone value. Not part of the analysis workflow. |
| `records/` | optional | Decision Records + `_index.md`. Created on first Standard/Deep analysis. |
