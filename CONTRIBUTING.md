# Contributing

Thanks for helping make the book better. Typos, broken examples, technical
inaccuracies, and translations are all welcome.

## How to contribute

1. Fork the repository and create a branch.
2. Edit the Markdown source (`BOOK.pt-BR.md` and/or `BOOK.en.md`). **Never**
   hand-edit anything under `typst/` or `dist/` — those are generated.
3. Follow the conventions in [CLAUDE.md](CLAUDE.md): heading hierarchy, Mermaid
   with the kit's semantic node classes (no emojis, no inline `style`), and
   consistency with the TaskFlow Pro example.
4. Build both languages (`bash kit/scripts/build.sh --all` and the same with
   `SOURCE_MD=BOOK.en.md`) and confirm they compile.
5. Open a PR and fill in the template.

For anything bigger than a few paragraphs (a new section, a new chapter),
**open an issue first** so we can agree on scope before you spend the time.

## Licensing of contributions

This repository has two licenses (see [README](README.md#license)):

- **Book content** (`BOOK.*.md`, diagrams, text): [CC BY-NC-SA 4.0](LICENSE).
- **SDD Kit** (`sdd-kit/`): [MIT](sdd-kit/LICENSE).

By opening a PR you confirm that:

1. **You wrote it** (or have the right to submit it), and it does not copy
   text, code, or images from a source that forbids it.
2. Your contribution is licensed under the license of the part you changed
   (above).
3. You also grant the author, Felipe Fontoura, a perpetual, worldwide,
   non-exclusive right to use, adapt, and **sell** your contribution as part of
   the book — in print, EPUB, PDF, or any other edition — and to relicense it.
   This is what lets the book stay open on GitHub and still be sold. You keep
   the copyright on what you wrote.

The PR template has a single checkbox for this. If you can't tick it, say so in
the PR and we'll find another way (usually: open an issue and the author
rewrites the point in their own words).

## Credit

Every merged PR adds you to [CONTRIBUTORS.md](CONTRIBUTORS.md)
automatically, with the kind of contribution. Substantial ones (translations,
new sections, in-depth reviews) are also credited by name in the book's
acknowledgments, unless you prefer not to be.

## Conduct

Be kind and specific. Critique the text, not the person.
