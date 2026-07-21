You are a Spec-Driven Development (SDD) assistant.

The user wants to initialize the SDD structure in this project.

## What to do

### 1. Create directory structure

Create the following directories (if they don't exist):
- `.claude/specs/`
- `.claude/steering/` (if it doesn't exist)
- `.claude/templates/` (if it doesn't exist)

### 2. Create INDEX.md

Create the file `.claude/specs/INDEX.md` with the content:

```markdown
# SDD — Feature Index

| ID  | Feature | Status | Last Updated |
|-----|---------|--------|--------------|
| — | No features created yet | — | — |
```

### 3. Verify support files

Check if the following exist:
- `.claude/steering/sdd-workflow.md` — if it doesn't exist, tell the user to copy it from sdd-kit
- `.claude/templates/` with templates (plan.md, prd.md, spec.md, tasks.md) — if they don't exist, tell the user

### 4. Welcome message

Display the following message:

```
SDD structure initialized successfully!

📁 Structure created:
  .claude/specs/           → Feature artifacts
  .claude/specs/INDEX.md   → Feature index

📋 Available commands:

  /sdd:plan [intent]       → Map product features and roadmap
  /sdd:prd [feature]       → Create PRD (requirements) for a feature
  /sdd:spec [feature]      → Create technical spec (design)
  /sdd:code [feature]      → Plan tasks and implement
  /sdd:review [feature]    → Review against PRD + spec
  /sdd:status              → Status dashboard

🚀 Next step:
  Use /sdd:plan to define the product vision and map features.
  Or use /sdd:prd [feature] to create the PRD for a specific feature.
```

## Rules

- DO NOT overwrite files that already exist
- DO NOT create template or steering files — only check if they exist
- Be concise in the output
