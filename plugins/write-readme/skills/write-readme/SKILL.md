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

A good README is the front door to your project. It determines whether someone uses your project, contributes to it, or walks away confused. This skill helps you create READMEs that are clear, concise, and structured — whether starting from scratch or improving what's already there.

## Checklist

Create a task for each of these items and complete them in order:

1. **Determine mode** — is this a new README or an improvement to an existing one?
2. **Read reference docs** — read both `references/docs/writing-principles.md` and `references/docs/adapting-by-project-type.md` before proceeding
3. **Analyze the project** — understand what the project does, why it exists, how it's built, and who it's for
4. **Determine project type and audience** — library, CLI, web app, API, internal tool, etc.
5. **Present findings to user** — confirm project type, audience, motivation, and planned structure before writing
6. **Draft the README** — write it using the structure template and applying the guidance from the reference docs
7. **Self-review** — read the first 10 lines, verify Quick Start, check for staleness, trim
8. **Review with user** — present the draft and ask for feedback
9. **Finalize** — apply feedback, do a final staleness pass, write the file

## Phase 1: Understand the Project

Before writing anything, build a mental model of the project.

### New README (no existing README, or user wants to start fresh)

- Read build/config files (`package.json`, `Cargo.toml`, `pyproject.toml`, `go.mod`, `build.gradle`) to understand dependencies, scripts, and metadata
- Glob for entry points (`src/main.*`, `src/index.*`, `src/lib.*`, `cmd/`, `app/`)
- Skim 3-5 core source files to understand what the project actually does
- Check for existing docs (`docs/`, `wiki/`, `.github/`, `CONTRIBUTING.md`) — tone and depth signals the intended audience
- Look at CI config (`.github/workflows/`, `Jenkinsfile`) for build/test commands
- Check for Docker files, environment configs, or setup scripts
- Check for a `LICENSE` file — signals open-source and affects which sections to include

**Identify the "why":** What problem does this solve? Why not use an existing tool? This is the most commonly missing section and the most important for adoption.

**Infer the audience:** Internal or public? Library consumers or app users? Beginners or experienced developers? Signals: LICENSE (open-source), CONTRIBUTING.md (community project), Docker setup (ops audience).

### Improving an Existing README

- **Staleness check first** — verify every command, path, version, and feature against the actual project state. A wrong README is worse than no README.
- Read the current README fully, then do the same project analysis as above
- Identify gaps: what's missing, outdated, or unclear?

### Present Findings

Before writing: "This looks like a [type] project that [does X] because [why]. Target audience: [Y]. Gaps I found: [list]. Planned structure: [outline]. Does this sound right?"

Wait for confirmation. Call out any staleness — users often don't realize their README has drifted.

## Phase 2: Write the README

### Required Reading Before Drafting

!IMPORTANT: Read these three files before writing a single line of the README:

1. **`references/readme-template.md`** — the canonical structure template to follow
2. **`references/docs/writing-principles.md`** — writing style, header formatting, badges, dividers, tables, and what to avoid
3. **`references/docs/adapting-by-project-type.md`** — how to adapt the template by project type (CLI, library, API, web app, monorepo)

These files are not optional. The skill base directory is the directory containing this SKILL.md file — use the Read tool with the full path to load each one.

## Phase 3: Review and Finalize

After drafting, do a self-review before presenting to the user:

1. **Read the first 10 lines.** If a stranger can't tell what this project does and why they should care within 10 lines, rewrite the opening.
2. **Try the Quick Start mentally.** Are any dependencies missing? Are commands correct? Would a fresh checkout actually work?
3. **Check for staleness.** Verify every command, path, version, and feature name against the actual project state. Stale information erodes trust.
4. **Trim.** Ask for every section: "Does this help the reader do something or understand something important?" Cut what doesn't.

Present the draft and explicitly ask: "Does this cover everything? Anything missing or anything you'd remove?"

## When the User Asks to Improve an Existing README

Focus on what's wrong or missing. Common gaps:

- **Weak or buried opening** — can't tell what the project does within 10 lines
- **Missing "Why" section** — no motivation or comparison to alternatives
- **Missing Quick Start** — the #1 structural gap
- **Outdated instructions** — verify against the actual project
- **Wall of text** — break into sections, convert paragraphs to bullets
- **No examples** — add working code with expected output
- **Too long** — move content to `docs/` and leave a link
- **Missing context** — doesn't explain what the project is or why it exists

Don't rewrite what's already good. Focus changes on the gaps.
