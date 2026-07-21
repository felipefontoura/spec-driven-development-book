# SDD Kit — Spec-Driven Development for Claude Code

The companion kit to the book **Spec-Driven Development: The Definitive Guide to Building Software with AI Agents**. It turns the method from the book into an enforceable workflow: explicit artifacts, human approval gates, and a `.status` file that is the single source of truth for what is approved.

```text
IDEA → PLAN → REQUIREMENTS → DESIGN → TASKS → EXEC → REVIEW
```

This is the Claude Code port of [`@felipefontoura/pi-sdd-kit`](https://github.com/felipefontoura/pi-sdd-kit). The skills are plain `SKILL.md` files following the [Agent Skills](https://agentskills.io) standard, so they also run on Pi, Codex, Cursor, and the other 30-plus tools that implement the format.

## Install

Copy `.claude/` into your project root (merge with an existing `.claude/` if you have one):

```bash
cp -r sdd-kit/.claude/ your-project/.claude/
cp -r sdd-kit/templates/ your-project/.ai-templates/   # optional: user-facing copies
```

Reload Claude Code. The skills register as `/sdd-*` slash commands.

## Skills

| Command | Phase | What it does |
|---------|-------|--------------|
| `/sdd-init` | — | Creates the `.ai/` workspace (steering + sdd) |
| `/sdd-steering` | — | Creates/updates durable context (product, tech-stack, conventions, principles) |
| `/sdd-idea` | IDEA | Explores a raw idea before committing |
| `/sdd-plan` | PLAN | Product plan: features, phases, dependencies |
| `/sdd-prd` | REQUIREMENTS | `requirements.md` — WHAT and WHY, EARS, negative scope |
| `/sdd-spec` | DESIGN | `design.md` — HOW, with the requirements-mapping table |
| `/sdd-tasks` | TASKS | `tasks.md` + implementation readiness check |
| `/sdd-exec` | IMPLEMENTATION | Implements ONE approved task, with verification |
| `/sdd-review` | REVIEW | `review.md` — verification against the approved artifacts |
| `/sdd-status` | — | Read-only dashboard: state, blockers, next safe action |

## Sub-agents

Three narrow agents in `.claude/agents/`, each with one job and one constraint:

- **architect** — produces requirements and design; never writes code.
- **implementer** — implements one approved task; STOPs on ambiguity.
- **reviewer** — reviews against the spec in a fresh context; reports gaps only.

## The rules the kit enforces

- The feature number comes from the filesystem (`max + 1`), never from memory.
- `.status` is the single source of truth. File existence is not approval.
- A draft never unlocks a gate — only explicit human approval does.
- No code before `tasks:approved`.
- No completion claim without fresh verification evidence.
- Artifacts are written in the language of the conversation; identifiers (status values, IDs, paths, commands) stay canonical in English.

## Workspace layout

```text
.ai/
  steering/          product.md · tech-stack.md · conventions.md · principles.md
  sdd/
    INDEX.md · PLAN.md
    specs/NNN-feature-name/
      .status · requirements.md · design.md · tasks.md · review.md
```

## Status values

```text
idea:exploring · idea:captured
plan:draft · plan:approved
requirements:draft · requirements:approved
design:draft · design:approved
tasks:draft · tasks:approved
implementation:in-progress · implementation:done
review:done
```

## License

MIT.
