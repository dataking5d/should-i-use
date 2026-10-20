# Investigation playbooks

Pick the block matching the resource type. Each block sets **what to look at** and **which
costs actually bear on the decision**. These are not scorecards — never tally them.

A type that isn't here: add a block. Do not force-fit an existing one.

**These blocks list what *can* be checked, not what must be.** Rule 0 (the sufficiency gate) runs
throughout: stop at the point where the verdict is settled, and skip any item that cannot change
the action.

---

## Always check (every type)

1. 环境兼容性 — Windows 11 / PowerShell
2. 与我已有能力的重叠程度 — verified, not recalled, and judged for **availability** rather than
   mere presence (workflow step 4, SKILL.md rule 5)
3. 新增依赖与系统复杂度
4. 后续维护成本 — who pays it, for how long
5. 安全 / 权限 / 数据风险
6. 退出、替换和回滚成本

Then add **only** the type-specific axes below that genuinely bear on the decision:

Windows compatibility · Claude Code compatibility · Codex compatibility · MCP protocol
compatibility · model dependency · vendor lock-in · API/token cost · GPU/CPU/RAM · storage ·
DB/schema migration · network dependency · deployment complexity · latency · context
consumption · prompt pollution · agent orchestration complexity · dependency conflicts ·
license restrictions · security surface · data privacy

---

## Claude Code / Codex / other agent Skills
**Answer:** does it fire on the right trigger, and does it pollute my context when it doesn't?
**Look at:** frontmatter `description` (trigger precision — over-broad triggers are a real cost),
whether it force-loads references, supporting-file structure, whether it assumes a runtime I
don't use, whether it needs `allowed-tools` or Bash.
**Cost:** context consumption, prompt pollution, trigger collision with skills I already own
(a skill that fires on the same phrases as mine degrades both), portability to Codex.

## MCP Server
**Answer:** what does the tool surface actually expose, and what can it reach?
**Look at:** tool list and schemas, auth model, transport (stdio/SSE/HTTP), how many tools it
injects into every conversation, install/registration method, whether it needs a local service
or DB.
**Cost:** context consumption (tool definitions are paid on *every* turn), permission surface,
network/data egress, overlapping tools I already have registered, registration fragility.

## Agent / Multi-Agent framework
**Answer:** how invasive is it, and can I take part of it?
**Look at:** orchestration model, tool routing, context/memory management, model binding,
whether the abstractions are separable from the runtime, state format, examples and tests.
**Cost:** architecture intrusion into my stable main chain, vendor/model lock-in, dependency
weight, migration cost if I adopt and later leave, orchestration complexity I must now debug.

## Memory / RAG / Knowledge base
**Answer:** does its retrieval mechanism beat what I already run, on *my* data?
**Look at:** indexing and chunking strategy, embedding model and its dependency, whether
retrieval is local or API-bound, data lifecycle (ingest → update → delete → export), how recall
is injected into context, storage format and portability.
**Cost:** overlap with my existing knowledge bases, embedding/API cost, storage, data privacy,
context injection volume, lock-in of the store format, backup/restore.

## Database / Vector database
**Answer:** is the data model right, and can I get my data back out?
**Look at:** data model, query language/API, index types, persistence guarantees, migration
path from whatever I run now, backup/restore, operational prerequisites.
**Cost:** deployment complexity, schema migration, storage growth, vendor lock-in, exit cost
(this is long-lived infrastructure — weight maintainability and reversibility heavily).

## Library / SDK / CLI / API wrapper / npm·PyPI package
**Answer:** is the API pleasant and is the maintenance alive?
**Look at:** public API shape, dependency tree (and its conflicts with my env), versioning and
breaking-change history, release cadence vs. its own nature, test coverage, whether it's a thin
wrapper over something I could call directly.
**Cost:** dependency conflicts, install-script behaviour on Windows, maintenance risk,
replacement cost, whether an API wrapper just adds a layer over an HTTP call I could make.

## Self-hosted application / full product
**Answer:** is the operational burden proportionate to the value?
**Look at:** deployment method (docker/compose/binary), external service dependencies,
resource footprint, auth and multi-user model, upgrade path, data export, whether it wants to
own a port and a database.
**Cost:** deployment + ongoing ops, resource usage, security surface, upgrade/rollback
difficulty, who fixes it at 2am.

## Model / Hugging Face project / dataset
**Answer:** can I actually run it, and is it actually better than my current default?
**Look at:** licence and usage restrictions, parameter size vs. my hardware, quantization,
inference stack, benchmark methodology (not just the headline number), training data
provenance, whether it needs GPU.
**Cost:** GPU/CPU/RAM, storage, latency, model download/versioning, licence restrictions,
cost of switching away later.

## Prompt / workflow / template collection
**Answer:** is there a transferable idea, or just prose?
**Look at:** whether the prompts encode a real mechanism or restate common sense, structure
(schema, tool contract, routing), whether it depends on a model I don't use.
**Cost:** near zero to adopt — so the bar is "is there a genuinely better idea here", and the
honest answer is often no.

## Paper / technical article
**Answer:** is there a claim I can use, and does the evidence support it?
**Look at:** the actual claim vs. the abstract's framing, benchmark setup and baselines,
whether the result depends on compute/data I don't have, code availability, whether the
technique is separable from its implementation.
**Cost:** reproduction effort, hidden compute assumptions. Treat every headline number as
`[项目方声明]` until you read the setup.

## Documentation / site only (no source visible)
**Answer:** can I verify anything at all?
**Look at:** whether real docs exist beyond a landing page, whether source is available.
**Cost:** unverifiable claims cap the maximum verdict at **需要更多证据**. Say so rather than
producing a confident read of marketing copy.

---

## Two things no playbook asks for

- **A fix for a project you are not adopting.** Once the verdict is 不采用, a compatibility defect
  is a line in the risk list, not a fork plan. Only the *difficulty* of fixing it can matter, and
  only if that difficulty would change the verdict.
- **A market-wide competitor search.** The cheaper-path check (workflow step 5) compares against
  what the user already has and against the obvious primitive — a CLI, a library, a twenty-line
  script. "Something better might exist" is not a finding, and searching for it is scope creep.
