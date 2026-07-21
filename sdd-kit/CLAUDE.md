# SDD Kit — instructions for Claude

This folder is the reader-facing SDD kit that ships with the book (Appendix B). It is the Claude Code port of `pi-sdd-kit`.

## Layout

```text
sdd-kit/
├── .claude/
│   ├── skills/            # 10 SKILL.md skills (/sdd-init … /sdd-status)
│   │   └── _shared/references/   # sdd-practical.md, templates.md (loaded by skills)
│   └── agents/            # architect, implementer, reviewer sub-agents
├── templates/            # user-facing copy-paste templates
├── README.md
└── CLAUDE.md             # this file
```

## Editing rules

- Skills use the **Agent Skills** `SKILL.md` format (`name` + `description` frontmatter). Keep the description precise about when to use AND when not to — Claude uses it to auto-invoke.
- Invocation in Claude Code is `/sdd-<name>` (not Pi's `/skill:sdd-<name>`). Do not reintroduce the `/skill:` prefix.
- Skills load their shared rules from `_shared/references/`; keep those two files as the single source of the workflow rules.
- The kit and the book must agree: the pipeline is `IDEA → PLAN → REQUIREMENTS → DESIGN → TASKS → EXEC → REVIEW`, `.status` is the only gate, and the numbering comes from the filesystem. If you change one, change the other.
- This kit is downstream of the book's method — when in doubt, the book's Part I is canonical.
