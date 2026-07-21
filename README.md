# 📘 Spec-Driven Development

> **The Definitive Guide to Building Products with AI**

A complete and practical ebook on how to use specifications to maximize productivity with AI agents like Claude Code, Cursor, and Copilot.

---

## 🎯 What You'll Learn

- **SDD Methodology** — Requirements → Design → Tasks → Implementation
- **Spec Structure** — Ready-to-use templates for immediate use
- **Practical Workflow** — Slash commands and specialized sub-agents
- **Complete Project** — TaskFlow Pro as an end-to-end example

## 📖 Contents

### Part I: Fundamentals

1. The Problem Nobody Talks About
2. What is Spec-Driven Development
3. Anatomy of a Specification
4. Writing Effective Specs

### Part II: Complete Specs (TaskFlow Pro Project)

1. The TaskFlow Pro Project
2. Authentication Spec
3. Workspaces Spec
4. Tasks Spec
5. Automations Spec
6. Notifications Spec

### Part III: Advanced Techniques

1. Custom Slash Commands
2. Specialized Sub-agents
3. Conclusion

### Appendices

- Ready-to-Use Templates (requirements.md, design.md, tasks.md)
- Glossary
- Spec Index

## 🛠️ Example Project Stack

The ebook uses **TaskFlow Pro** as an example project — a collaborative task management system:

```
Turborepo + Next.js 14 + Fastify + Prisma + shadcn/ui
Socket.io (real-time) + BullMQ (queues) + Redis
```

**Features covered:**

- ✅ Authentication (email/password + magic links)
- ✅ Collaborative workspaces with roles
- ✅ Tasks with subtasks, tags, due dates
- ✅ Automations (when X happens, do Y)
- ✅ Real-time notifications

## 🚀 How to Use

### Online Reading

Open the [`BOOK.en.md`](./BOOK.en.md) or [`BOOK.pt-BR.md`](./BOOK.pt-BR.md) file directly on GitHub.

### Download

```bash
git clone https://github.com/SEU_USER/spec-driven-ebook.git
```

### Practical Application

1. Copy the templates from Appendix A
2. Adapt to your project
3. Follow the workflow: Requirements → Design → Tasks → Code

## Building the book (PDF + EPUB)

Print-ready PDF and reflowable EPUB are generated from the Markdown source by the **base25-book-kit** engine — a git submodule mounted at `kit/`. The PDF is sized 6×9" for Kindle Print; the EPUB is EPUB 3 with a 1600×2560 cover, ready for KDP digital upload. Every book-specific string (title, cover, copyright) lives in `book.config.json`.

### Sources

- `BOOK.pt-BR.md` — Brazilian Portuguese (primary; targets KDP-BR and base25.so)
- `BOOK.en.md` — English

Either file is optional; the build runs for whichever exists.

### Local build

```bash
git submodule update --init        # fetches the kit engine
mise install                       # installs Node + Typst
bash kit/scripts/build.sh          # PDF + EPUB (default; installs kit npm deps on first run)
bash kit/scripts/build.sh --pdf    # PDF only
bash kit/scripts/build.sh --epub   # EPUB only
```

Output:

- `dist/book-{pt-br,en}.pdf` — print-ready interior
- `dist/book-{pt-br,en}.epub` — reflowable ebook (KDP upload)
- `dist/cover-{pt-br,en}.png` — 1600×2560 cover (KDP separate upload)

Force a specific source with `SOURCE_MD=BOOK.en.md bash kit/scripts/build.sh`.

### Pipeline

```
BOOK.{pt-BR,en}.md + book.config.json
  ├─ kit/scripts/render-mermaid.mjs   → typst/assets/diagrams/*.svg  (one-shot via mmdc)
  │
  ├─ kit/scripts/md-to-typst.mjs      → typst/chapters/*.typ + chapters.typ
  │  typst compile kit/typst/book.typ → dist/book-*.pdf
  │
  ├─ typst compile kit/typst/cover.typ → dist/cover-*.png   (1600×2560 @ 250ppi)
  │
  └─ kit/scripts/md-to-epub.mjs       → dist/book-*.epub
     (marked.parse() per chapter, html-to-epub assembles)
```

The engine (scripts, Typst template, cover renderer, fonts, EPUB stylesheet) is shared across the Base25 books via the submodule — to change typography or pipeline behaviour, edit in the `base25-book-kit` repo and bump the submodule pointer here (`git submodule update --remote kit`).

### Publishing

- **KDP (Kindle)**: upload the EPUB and the cover PNG. KDP converts the EPUB to KFX internally; the cover stays as the marketplace thumbnail.
- **KDP Print** (paperback): upload the PDF (6×9") and the cover PNG. KDP will ask for a full wraparound cover for print — the front cover from `dist/cover-*.png` is reusable, but spine + back need a separate file.
- **base25.so / direct**: ship the PDF + EPUB as a bundle. Skip KDP Select (its 90-day digital exclusivity prevents selling the EPUB anywhere else).

### CI

`.github/workflows/build.yml` builds PT and EN in parallel on every push and PR, and uploads PDF + EPUB + cover as artifacts. Pushing a `v*` tag attaches all three to a GitHub Release.

## 💡 Why This Ebook?

> "AI agents are extraordinarily fast masons. But without the house blueprint, they build the bathroom where the kitchen should be."

Experienced developers know how to code. The challenge with AI is **communicating what to code**. This ebook teaches exactly that.

## 📊 Who It's For

- Developers using Claude Code, Cursor, Copilot
- Teams that want to structure their work with AI
- Anyone tired of redoing code because "the AI didn't understand"

## 🤝 Contributing

Found an error? Want to suggest an improvement?

1. Open an Issue
2. Or submit a PR
