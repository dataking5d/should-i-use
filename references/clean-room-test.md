# Clean-room test — maintainer validation protocol

**Not part of the analysis workflow.** This document is for validating the skill itself. An agent
invoked by a user to analyze a project must never run this protocol.

## What it measures

Two different numbers, which must never be confused:

| Metric | Question | What it decides |
|---|---|---|
| **Local marginal value** | Over and above *my* host environment (global rules, registry, other skills, existing records), what does this skill add? | Whether it earns its place in *my* setup |
| **Standalone value** | With a bare agent and no environment support at all, can this skill turn "project analysis" into "adoption decision for a specific user"? | What a **published** skill must contain |

**Local marginal value must never be used to decide what a published skill keeps or drops.** A rule
that duplicates the author's global instructions is redundant *locally* and essential *standalone*.
The previous version of this document's author made exactly that error; do not repeat it.

## The isolation requirement

Both arms must run with **none** of the following:

- the author's global instruction file (`CLAUDE.md` or equivalent)
- any project registry or ecosystem index
- any other skills, agents, or plugins
- any existing Decision Records
- any personal decision profile (`profile.md`)

If any of these leaks into either arm, the run measures local marginal value, not standalone value,
and the result is not usable for deciding the skill's content.

**Achieving isolation is environment-specific and must be verified per run, not assumed.** Hosts
commonly inject global instruction files into subagents automatically. Before trusting a result,
confirm the arm's transcript shows no access to the excluded material. Options, in order of
reliability: a separate machine or container with a clean home directory; the Agent SDK with a
minimal system prompt; a fresh user profile. Copying the skill into a temp directory is **not**
sufficient on its own if the host still injects global rules.

## Protocol

**RED-clean** — bare agent, no skill, no overlay. Task:

> 「结合我的情况，分析这个项目对我有没有价值、是否值得使用：<URL>」

**GREEN-clean** — same bare agent, same task, with `should-i-use` available (and nothing else).

Same project in both arms. Different projects confound project complexity with skill effect.

Constraints on both arms: identical task text, verbatim; neither is told what is being measured;
a web-fetch budget high enough to reach the evidence (8 is a reasonable floor). Use a project in a
category where the skill's core hypothesis is testable — i.e. one that plausibly overlaps with
capability the user might already have. A project nobody could plausibly own measures nothing.

Repeat across at least three projects of different types before drawing conclusions. Single runs on
a single project are anecdotes; the first evaluation of this skill was one project, one model.

## Scoring — behaviour, not format

Do not penalise the baseline for lacking the skill's report structure. Score these:

| # | Behaviour | Pass looks like |
|---|---|---|
| 1 | Establishes user context | Derives or asks about the user's situation instead of writing for an anonymous reader |
| 2 | Project quality ≠ fit | Distinguishes "well made" from "worth adopting here" |
| 3 | Whole vs component split | Two separate verdicts |
| 4 | Real increment, not feature list | Named comparison of existing capability vs new, including "merely a different implementation" |
| 5 | Evidence grading | Distinguishes verified fact / vendor claim / inference / unknown |
| 6 | Active verification | Goes to source, docs or issue tracker to settle a load-bearing question |
| 7 | Cost against increment | Includes maintenance and exit/rollback cost, and answers whether cost cancels increment |
| 8 | Cheaper-path check | Asks whether the same increment is available more cheaply |
| 9 | Permits inaction | Can conclude 无需行动 when warranted |
| 10 | Not driven by surface signals | Stars/hype absent from the deciding reasoning |

## Interpreting the result

- **Standalone value is high** if GREEN-clean shows several of behaviours 1–10 that RED-clean lacks,
  especially 1, 3, 8, 9 — the ones that turn analysis into a decision.
- **Standalone value is low** if RED-clean already does most of them. Then the skill's content, not
  just its length, needs rethinking: keep only what changed behaviour.
- **Either way, do not delete a core rule because the author's own environment already enforces it.**
  That is what the overlay is for.
