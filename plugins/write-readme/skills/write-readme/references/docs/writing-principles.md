# Writing Principles

## Write for Newcomers

The primary audience is someone who just found this repo for the first time. Don't assume they know the domain, the
team's conventions, or why the project exists. Write as if the reader has zero context. Define terms on first use. Spell
out acronyms.

## Be Complete Before Being Concise

A README that covers the essentials is the goal — but missing content is worse than extra content. When in doubt,
include it. Edge cases and deep dives belong in `docs/` or a wiki (link to them), but the core path to "it works" must
be complete in the README.

## Every Sentence Should Earn Its Place

Don't repeat information. If a section isn't pulling its weight, cut it. But cut only after confirming everything needed
to get started is covered.

## Be Structured

Put the most important information first — what the project is and why someone should care. Use headings so people can
scan. Use bullet lists and code blocks instead of dense paragraphs. Keep formatting consistent.

## Show Real Examples with Expected Output

Code examples should be runnable and realistic, not pseudocode. Always include expected output so the reader knows when
it worked.

## Prefer Tables for Structured Information

Whenever items have consistent attributes, use a table instead of prose or bullets:

| Use case                | Table columns                            |
|-------------------------|------------------------------------------|
| CLI flags               | Flag, Default, Description               |
| Environment variables   | Variable, Required, Default, Description |
| API endpoints           | Method, Path, Description                |
| Scripts                 | Command, Description                     |
| Architecture/tech stack | Layer, Tech, Port                        |

## What to Avoid

- **Don't pad with meaningless badges.** Badges should convey real information (build status, version, license). Skip
  purely decorative ones.
- **Don't write a tutorial in the README.** Tutorials, deep dives, and advanced usage belong in `docs/` or a wiki. Link
  to them.
- **Don't include every option.** Document the common path. Edge cases belong in reference docs.
- **Don't let it get stale.** A wrong README is worse than no README. If install instructions have changed, update them.
- **Don't assume context.** Don't reference internal jargon or Slack channels without explanation (unless it's an
  internal project).

## Header Formatting

The title block is centered and has three parts:

1. **Project name** as an H1, centered
2. **Short description** directly beneath — one or two sentences
3. **Badges** below — build status, version, license. Skip decorative badges.
4. **Navigation links** — inline links to key sections (Installation, Usage, Contributing)

Example:

```markdown
<div align="center">

# datakit

Lightweight, zero-dependency data validation and transformation for TypeScript.

[![npm](https://img.shields.io/npm/v/datakit)](https://npmjs.com/package/datakit) [![License](https://img.shields.io/badge/license-Apache--2.0-blue)](#license)

[Installation](#installation) · [Quick Start](#quick-start) · [Contributing](#contributing)

</div>
```

## Section Dividers

Use horizontal rules (`---`) between major sections for visual breathing room. Place `---` after every `##` section's
content, before the next heading.

## Links

Use **relative links** for internal references — they work consistently across clones and forks:

- ✅ `[Contributing](CONTRIBUTING.md)` or `[License](LICENSE)`
- ❌ `https://github.com/org/repo/blob/main/CONTRIBUTING.md`

Use absolute URLs only for external resources (shields.io, npm, docs sites).
