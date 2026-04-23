---
name: write-readme
description: >
  Use when the user wants to create, improve, or rewrite a README file for any
  project. Also use when the README is outdated, incomplete, or unclear. Do NOT
  trigger for writing CLAUDE.md files, rule files, API reference docs, changelogs,
  or CONTRIBUTING.md files.
allowed-tools: Read, Write, Edit, Glob, Grep, Bash, Agent
---

# Writing README Files

A good README is the front door to your project. It determines whether someone uses your project, contributes to it, or walks away confused. This skill helps you create READMEs that are clear, concise, and structured — whether you're starting from scratch or improving what's already there.

## Checklist

Create a task for each of these items and complete them in order:

1. **Determine mode** — is this a new README or an improvement to an existing one?
2. **Analyze the project** — understand what the project does, why it exists, how it's built, and who it's for
3. **Determine project type and audience** — library, CLI, web app, API, internal tool, etc.
4. **Present findings to user** — confirm project type, audience, motivation, and planned structure before writing
5. **Draft the README** — write it using the structure template below
6. **Self-review** — read the first 10 lines, verify Quick Start, check for staleness, trim
7. **Review with user** — present the draft and ask for feedback
8. **Finalize** — apply feedback, do a final staleness pass, write the file

## Phase 1: Understand the Project

Before writing anything, build a mental model of the project. The depth of analysis depends on the mode:

### New README (no existing README, or user wants to start fresh)

- Read `package.json`, `Cargo.toml`, `pyproject.toml`, `go.mod`, `build.gradle`, or whatever build/config files exist to understand dependencies, scripts, and project metadata
- Glob for entry points (`src/main.*`, `src/index.*`, `src/lib.*`, `cmd/`, `app/`)
- Skim 3-5 core source files to understand what the project actually does
- Check for existing docs (`docs/`, `wiki/`, `.github/`, `CONTRIBUTING.md`) — their tone and depth signals the intended audience's technical level
- Look at CI config (`.github/workflows/`, `Jenkinsfile`, `.gitlab-ci.yml`) for build/test commands
- Check for Docker files, environment configs, or setup scripts
- Check for a `LICENSE` file — its presence signals open-source and affects tone and what sections to include

**Identify the "why":** Beyond what the project does technically, answer: what problem does it solve? Why not use an existing tool? This is what goes in the opening description and is the most common gap in weak READMEs.

**Infer the audience:** Is this internal or public? Library consumers or app users? Beginners or experienced developers? Signals: presence of a LICENSE (open-source), CONTRIBUTING.md (community project), Docker setup (ops audience), test suite maturity.

### Improving an Existing README

- **Staleness check first** — this is the most critical step. A wrong README is worse than no README. Before analyzing gaps, verify every command, path, version, and feature mentioned in the existing README against the actual project state. Cross-reference install commands against `package.json` scripts, listed features against source files, referenced paths against the actual directory structure.
- Read the current README fully
- Do the same project analysis as above
- Identify gaps: what's missing, outdated, or unclear?

### Present Findings

Before writing, tell the user what you've learned: "This looks like a [type] project that [does X] because [why it exists]. The target audience seems to be [Y]. I found these gaps: [list]. Here's what I'm planning to include — does this sound right?"

Wait for confirmation before drafting. Call out any staleness found — users often don't realize their README has drifted from the actual project.

## Phase 2: Write the README

### Writing Principles

These principles matter because they directly affect whether people actually read and use your documentation:

**Write for newcomers.** The primary audience is someone who just found this repo for the first time. Don't assume they know your domain, your team's conventions, or why your project exists. Write as if the reader has zero context. If a term might confuse newcomers, define it on first use. Spell out acronyms the first time.

**Be complete before being concise.** A README that covers the essentials is the goal — but missing content is worse than extra content. When in doubt, include it. Edge cases and deep dives belong in `docs/` or a wiki (link to them), but the core path to "it works" must be complete in the README.

**Every sentence should earn its place.** Don't repeat information. If a section isn't pulling its weight, cut it. But cut after confirming everything needed to get started is covered.

**Be structured.** Put the most important information first — what the project is and why someone should care. Use headings so people can scan. Use bullet lists and code blocks instead of dense paragraphs. Keep formatting consistent.

**Show real examples with expected output.** Code examples should be runnable and realistic, not pseudocode. Always include expected output so the reader knows when it worked. This is what turns a confusing README into a useful one.

### Structure Template

Adapt this structure to the project. Not every section applies to every project — use judgment about what to include. The order matters: it's designed so readers get the most important information first.

```markdown
<div align="center">

# Project Name

One or two sentences: what this project does and why it exists. Answer the question
"why should I care?" immediately. Don't start with implementation details.

[![Badge](link)](#) [![Badge](link)](#)

</div>

---

## Features

Bullet list of key capabilities. Keep it scannable — one line per feature.
Skip this section for very simple projects where the description covers it.

---

## Installation

How to install — the reader's next question after knowing what the project offers.
- Install command(s) for the primary method (package manager, Docker, etc.)
- Prerequisites if any (runtime versions, system dependencies)
- Verification step ("run X to confirm it's working")

---

## Quick Start

The fastest path from zero to "it works." Assume the reader already installed.
This section should get someone running in under 2 minutes.

Include:
- Minimal usage example (working code, not pseudocode)
- Expected output so the reader knows it worked

---

## Usage

Show the most common use cases with real examples. For libraries, show the primary API.
For CLIs, show the most-used commands. For web apps, describe the main workflows.

Use code blocks with the correct language tag. Show realistic inputs and outputs,
not toy examples. If there are many use cases, cover the top 3-5 and link to full docs.

---

## Configuration

Document configuration options if they exist. A table works well for this:
| Option | Default | Description |
Use env var names, config file paths, or CLI flags — whatever the project uses.
Skip this section if there's nothing to configure.

---

## API Reference

For libraries: document the public API, or link to generated docs.
For APIs: link to OpenAPI spec, Postman collection, or hosted docs.
Skip for CLIs and apps — their "API" is covered in Usage.

---

## Development

How to set up a development environment and contribute:
- Clone and install dependencies
- How to run tests
- How to build
- Code style or linting setup
- Link to CONTRIBUTING.md if it exists

---

## Architecture

Brief overview of how the project is organized, for projects where it's not obvious.
A sentence or two per major module/directory. Helps new contributors orient themselves.
Skip for small or simple projects.

---

## Contributing

How to contribute — link to CONTRIBUTING.md if it exists, otherwise a brief note
on how to open issues or PRs. Include any requirements (tests must pass, code style, etc.).
Skip for internal tools where contribution is implicit.

---

## Authors / Maintainers

Who built and maintains this. For open-source projects: names, GitHub handles,
or a link to the contributors page. For internal tools: team name and contact channel.
Skip if maintainership is obvious from the repo context.

---

## Support

Where to get help — GitHub Issues, a Discord, a mailing list, or an internal Slack channel.
Include a link. Skip if contributing/issues process is already clear from context.

---

## Changelog

Link to CHANGELOG.md or release notes if they exist.
Skip if the project doesn't maintain a changelog.

---

## License

State the license and link to the LICENSE file.
```

### Title and Header

The title block is centered and serves as the project's first impression. It has three parts:

1. **Project name** as an H1, centered
2. **Short description** directly beneath — one or two sentences, no heading needed
3. **Badges** below the description — use shields.io or similar. Include badges that convey useful information: build status, latest version, license, language. Skip purely decorative badges.

Example:
```markdown
<div align="center">

# datakit

Lightweight, zero-dependency data validation and transformation for TypeScript.

[![npm](https://img.shields.io/npm/v/datakit)](https://npmjs.com/package/datakit) [![License](https://img.shields.io/badge/license-Apache--2.0-blue)](#license)

</div>
```

### Section Dividers

Use horizontal rules (`---`) between major sections. This gives the README visual breathing room and makes it easier to scan. Place a `---` after every top-level `##` section's content, before the next `##` heading.

### Links

Use **relative links** for internal references (other files in the repo) — they work consistently across clones and forks:
- ✅ `[Contributing](CONTRIBUTING.md)` or `[License](LICENSE)`
- ❌ `https://github.com/org/repo/blob/main/CONTRIBUTING.md`

Use absolute URLs only for external resources (shields.io, npm, docs sites).

### Prefer Tables for Structured Information

Whenever you have a list of items with consistent attributes (CLI flags, env vars, API endpoints, scripts, tech stack components), use a table instead of prose or bullet lists. Tables are denser, more scannable, and easier to reference. Examples:

- CLI flags: `| Flag | Default | Description |`
- Environment variables: `| Variable | Required | Default | Description |`
- API endpoints: `| Method | Path | Description |`
- Scripts: `| Command | Description |`
- Architecture/tech stack: `| Layer | Tech | Port |`

### Adapting the Template

The template above is a starting point. Adapt it based on project type:

**Libraries/packages**: Emphasize Installation, Usage (with API examples), and API Reference. Quick Start should show `npm install` / `pip install` / `cargo add` and a minimal code snippet.

**CLI tools**: Emphasize Quick Start (install + one command), Usage (common commands with flags), and Configuration. Consider a command reference table.

**Web applications / internal tools**: Lead with an Architecture table showing the tech stack, layers, and ports. Include an API endpoints table if the app exposes routes. Add a scripts table for common commands. Emphasize Configuration (environment variables table), Getting Started (docker-compose or equivalent), and Development setup split by frontend/backend. For internal projects, include context about what systems this integrates with and who to contact.

**APIs/services**: Emphasize Quick Start (how to call the API), Authentication, and link to full API docs. Show a curl example or equivalent. Include an endpoints table.

**Monorepos**: Add a project/package map near the top showing what lives where. Each sub-project may need its own README.

### What to Avoid

- **Don't pad with meaningless badges.** Badges should convey real information (build status, version, license). Skip purely decorative ones ("made with love", "powered by coffee").
- **Don't write a tutorial in the README.** The README gets people started. Tutorials, deep dives, and advanced usage belong in `docs/` or a wiki. Link to them.
- **Don't include every option.** Document the common path. Edge cases and exhaustive option lists belong in reference docs.
- **Don't let it get stale.** A wrong README is worse than no README. If install instructions have changed, update them. If a feature was removed, remove it from the README too.
- **Don't assume context.** Write as if the reader just found this repo for the first time. Don't reference internal jargon, team names, or Slack channels without explanation (unless it's an internal project where that context is shared).

## Phase 3: Review and Finalize

After drafting, do a self-review before presenting to the user:

1. **Read the first 10 lines.** If a stranger can't tell what this project does and why they should care within 10 lines, they won't read the rest. The opening is the single biggest determinant of whether someone continues. Rewrite until it hooks immediately.
2. **Try the Quick Start mentally.** Walk through the steps — are any dependencies missing? Are commands correct? Would a fresh checkout actually work?
3. **Check for staleness — this is the highest-priority check.** A wrong README is worse than no README. Verify every command, path, version, and feature name against the actual project state. Stale information actively misleads users and erodes trust.
4. **Trim.** Read every section and ask: "Does this help the reader do something or understand something important?" Cut anything that doesn't — but only after verifying completeness.

Present the draft to the user and explicitly ask: "Does this cover everything? Anything missing or anything you'd remove?"

## When the User Asks to Improve an Existing README

Focus the conversation on what's wrong or missing. Common improvement patterns:

- **Weak or buried opening**: The reader can't tell what the project does or why they should care within the first 10 lines. The opening is the most important thing — if it doesn't hook immediately, nothing else matters.
- **Missing Quick Start**: The #1 structural gap. Add one.
- **Outdated instructions**: Commands or paths that no longer work. Verify against the actual project. A wrong README is worse than no README.
- **Wall of text**: Break into sections with headings. Convert paragraphs to bullet lists where appropriate.
- **No examples**: Add working code examples with expected output to Usage.
- **Too long**: Identify content that belongs in separate docs and move it there, leaving a link.
- **Missing context**: The README doesn't explain what the project is or why it exists.

Don't rewrite what's already good. Focus changes on the gaps.
