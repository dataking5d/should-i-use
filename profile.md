# Environment overlay — OPTIONAL

> **This file is not required.** `should-i-use` must produce a complete, user-specific verdict
> without it. It exists only to make the analysis sharper and cheaper, by pre-answering things the
> skill would otherwise have to derive from the environment or ask the user about.
>
> Delete it, replace it, or leave it empty — the skill's behaviour is defined in `SKILL.md`, and
> nothing in `SKILL.md` may depend on this file's existence. If it is missing, the skill falls back
> to deriving context from the current environment and, only where the verdict would change, asking
> the user a minimum set of questions.

The file below is one filled-in instance of the template. Two parts:

- **Part A — decision profile.** Stable preferences that decide *what counts as value* for this user.
- **Part B — ecosystem map.** Where their existing capabilities actually live, so overlap can be
  verified instead of guessed.

---

## Template

```markdown
## Part A — decision profile
### Environment
- OS / shell / runtime constraints
- Primary agent runtime(s) and any in-progress migration
- Language preferences
### Decision principles
- What the user optimizes for; what they refuse to trade away
- Whether capability duplication is a top risk or not
- Tolerance for secondary development; appetite for complexity
### Not adoption reasons
- Signals that must never drive the verdict
### Legitimate conclusions
- Which verdicts this user accepts as first-class

## Part B — ecosystem map
| What | Where | How to read it |
|---|---|---|
```

Fill only what is true; leave sections out rather than writing filler. A short honest profile beats a
long speculative one — the skill treats every claim in here as a fact about the user.

---

## Part A — decision profile (this instance)

### Environment
- Primary environment: **Windows 11**, PowerShell first. Anything POSIX-only or Linux-first carries
  a compatibility cost that must be priced in explicitly, not waved at.
- Runs **Claude Code** as the main agent runtime while building out **Codex**-side capability. New
  capability should fit that ecosystem rather than create platform lock-in.
- Prefers **Python** for anything written from scratch.

### Decision principles
- Many agents, skills, MCP servers and AI projects already accumulated. **Capability duplication is
  the primary risk.** Default assumption: something close already exists. The burden of proof is on
  "this is genuinely new", not on "this is unnecessary".
- Reasonable secondary development is acceptable. "Not turnkey" is not by itself a rejection.
- Wants modular, pluggable, replaceable, degradable design.
- Values rollback. Will not break a stable main chain for a new technology.
- Cares about real capability increment, not feature count.
- "整体不用，但拆出优秀组件" is actively used.
- For long-lived infrastructure, maintainability and exit cost dominate.
- Scarcest resource is time. A capability that saves recurring time beats a technically superior one
  that costs a weekend to wire up.

### Not adoption reasons
Popularity, star count, README polish, feature-list length, newness of the tech stack, or author
reputation. These may be *context*; they are never the reason.

### Legitimate conclusions
"什么都不做" and "整体不采用，只吸收 X" are first-class. A report that ends in a recommendation
because an empty verdict felt unsatisfying has failed.

---

## Part B — ecosystem map (this instance)

Verify against these paths **before** claiming the user does or does not already have a capability.

| What | Where | How to read it |
|---|---|---|
| Skills already present | `~/.claude/skills\*\SKILL.md` | frontmatter `name` + `description` of each — this is the capability inventory |
| Skill experiments | `~/.claude/skill-definitions\` | |
| MCP servers registered | `~/.claude/.claude.json` → `mcpServers`, and `~/.claude/.mcp.json` | grep for server names |
| Projects | `~/.claude/all_my_projects\` and `~/.claude/project_registry.json` | the registry is the authoritative index (name / path / port / start command) |
| Plugin-provided skills | `~/.claude/plugins\` | marketplace skills count as capability too |
| Reusable scripts | `~/.claude/scripts\` | |
| Durable context | project `memory/` dirs (ACP projects) and `~/.claude/projects\*\memory\MEMORY.md` | |

### Rules for using Part B
- `project_registry.json` is a **protected file** — read it, never modify it.
- Project-level `CLAUDE.md` files describe each project's positioning. Read the relevant one when the
  decision touches that project's domain.
- **When the check is cheap, do the check.** A Glob or Grep over skill descriptions costs far less
  than being wrong about duplication.
- **A capability-inventory read is cheap; an audit of my roster is a different task.** Answering
  "do I already have this?" is a frontmatter scan. Finding broken cross-references, missing sibling
  skills or token-budget violations across all my skills is a separate job with its own scope.
  Check the inventory; do not fold an audit into an adoption analysis (SKILL.md rule 0,
  corollary 2).
- **找不到 ≠ 不存在.** Before writing `[未知]`, confirm the search scope was reasonable. Searching one
  or two guessed paths is not enough to conclude something is absent. Prefer an authoritative index
  (such as `project_registry.json`) over guessing directory names, and widen the search before
  concluding the user lacks a capability.
- **This map evidences presence — not availability, and not demand.** Finding a library inside one
  project proves the technology exists on this machine. It does not prove a capability that can be
  called on where it is needed (SKILL.md rule 5), and it says nothing about whether the user needs
  it (rule 14, Demand Confidence). Three separate findings; never collapse them into one.
- If a path is missing or unreadable, record that as `[未知]`. Never assume "empty", and never assume
  "the user probably has something like this".
- Listing the same *category* of tool is not proof of overlap. Compare the actual mechanism — what it
  does, on what data, triggered how — before declaring duplication.
