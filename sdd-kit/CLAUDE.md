# [Project Name]

## About
[Project description — 2-3 lines about what it is, who it's for, and what problem it solves]

## Stack
[List the project's tech stack here]

<!-- If there is a steering file with stack details, reference it:
Details in @.claude/steering/tech-stack.md
-->

## Development Workflow

This project uses **Spec-Driven Development (SDD)**.
See @.claude/steering/sdd-workflow.md

### Available Commands

| Command | Phase | Description |
|---------|-------|-------------|
| `/sdd:init` | Setup | Initialize SDD structure |
| `/sdd:plan [intent]` | Planning | Map features and roadmap |
| `/sdd:prd [feature]` | PRD | Create feature requirements |
| `/sdd:spec [feature]` | Spec | Create technical design |
| `/sdd:code [feature]` | Code | Plan tasks and implement |
| `/sdd:review [feature]` | Review | Review implementation |
| `/sdd:status` | Dashboard | Status of all features |

### Pipeline

```
/sdd:plan → /sdd:prd → /sdd:spec → /sdd:code → /sdd:review
```

Mandatory human approval between each phase.

## Critical Rules

- NEVER advance a phase without explicit approval
- ALWAYS validate inputs on the server
- ALWAYS use transactions for multi-table operations
- NEVER expose data across isolated contexts
