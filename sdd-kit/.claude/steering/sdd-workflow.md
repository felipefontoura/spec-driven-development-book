# SDD Workflow — Spec-Driven Development

## Philosophy

Spec-Driven Development (SDD) is a methodology where **clear specifications precede code**.
Each feature goes through a structured pipeline with mandatory human approval between phases.

The goal: eliminate ambiguity, reduce rework, and create a complete audit trail
from the "why" to the "how" of each decision.

---

## Complete Pipeline

```
PLAN → [approval] → PRD → [approval] → SPEC → [approval] → CODE → [approval] → REVIEW
```

| Phase | Artifact | Question it answers |
|-------|----------|---------------------|
| PLAN | `PLAN.md` | What are we building and why? |
| PRD | `requirements.md` | What exactly does this feature do? |
| SPEC | `design.md` | How will we implement it technically? |
| CODE | `tasks.md` + code | Implementation following the spec |
| REVIEW | report | Does the implementation meet PRD + spec? |

---

## Phase Gates

### Golden Rule
**NEVER advance a phase without explicit user approval.**

### Valid Transitions

```
plan:draft → plan:approved          (user approves PLAN.md)
prd:draft → prd:approved            (user approves requirements.md)
spec:draft → spec:approved          (user approves design.md)
code:planning → code:approved       (user approves tasks.md)
code:approved → code:in-progress    (implementation starts)
code:in-progress → code:done        (all tasks completed)
review:in-progress → review:done    (review approved)
```

### Blocks
- Cannot create PRD without PLAN existing (warns, but does not block)
- Cannot create SPEC without approved PRD (blocks)
- Cannot generate CODE without approved SPEC (blocks)
- Cannot do REVIEW without completed CODE (blocks)

---

## Status System

Each feature has a `.status` file in `.claude/specs/XXX-name/` containing the current state.

### Possible states:
```
plan:draft
plan:approved
prd:draft
prd:approved
spec:draft
spec:approved
code:planning
code:approved
code:in-progress
code:done
review:in-progress
review:done
```

### `.status` File
Simple format — a single line with the current state:
```
prd:approved
```

---

## Directory Structure

```
.claude/
├── specs/
│   ├── INDEX.md                    # Table of all features
│   ├── PLAN.md                     # Product plan
│   ├── 001-authentication/
│   │   ├── .status                 # Current state
│   │   ├── requirements.md         # PRD
│   │   ├── design.md              # Technical spec
│   │   └── tasks.md               # Task breakdown
│   └── 002-course-catalog/
│       ├── .status
│       └── requirements.md
├── steering/
│   └── sdd-workflow.md             # This file
├── templates/
│   ├── plan.md
│   ├── prd.md
│   ├── spec.md
│   └── tasks.md
└── commands/
    ├── sdd:init.md
    ├── sdd:plan.md
    ├── sdd:prd.md
    ├── sdd:spec.md
    ├── sdd:code.md
    ├── sdd:review.md
    └── sdd:status.md
```

---

## Spec Writing Principles

### 1. Be Specific
- Bad: "The system should be fast"
- Good: "The API must respond in under 200ms for p95"

### 2. Negative Scope
Always declare what will **NOT** be done. This prevents scope creep and aligns expectations.

### 3. Concrete Examples
Include request/response examples, input/output data, and usage scenarios.

### 4. Anticipate Questions
Add an FAQ section with questions the implementer would have.

### 5. Traceability
Each artifact must reference the previous one:
- PRD references PLAN.md
- SPEC references requirements.md
- TASKS references requirements.md + design.md
- REVIEW references all artifacts

### 6. EARS Format for Functional Requirements
```
WHEN [event] IF [condition] THE SYSTEM SHALL [action] SO THAT [result]
```

### 7. MoSCoW Classification
- **Must Have** — without this the feature doesn't work
- **Should Have** — important but has a workaround
- **Could Have** — desirable, implement if time permits
- **Won't Have** — explicitly out of scope for this version

---

## Clarification Questions

Before generating any artifact, ALWAYS ask clarification questions.
This ensures the output is relevant and reduces review iterations.

### For PLAN:
- What is the main problem?
- Who will use it?
- Does anything exist today?
- Timeline/technology constraints?
- Desired complexity level?

### For PRD:
- What problem does this feature solve?
- Who are the users?
- What are the 3-5 main flows?
- What edge cases and error scenarios?
- Performance requirements?
- What is OUT of scope?

### For SPEC:
- Present approaches with trade-offs
- Let the user choose before going into detail

---

## Diagrams

Use **Mermaid** for all diagrams:
- `flowchart` — flows and architectures
- `sequenceDiagram` — interactions between components
- `stateDiagram-v2` — state machines
- `erDiagram` — data models
- `graph TD/LR` — hierarchies

---

## Available Commands

| Command | Phase | Description |
|---------|-------|-------------|
| `/sdd:init` | Setup | Initialize SDD structure |
| `/sdd:plan [intent]` | Planning | Map features and roadmap |
| `/sdd:prd [feature]` | PRD | Create feature requirements |
| `/sdd:spec [feature]` | Spec | Create technical design |
| `/sdd:code [feature]` | Code | Plan tasks and implement |
| `/sdd:review [feature]` | Review | Review implementation |
| `/sdd:status` | Dashboard | Status of all features |
