<h1 align="center">
Claude Toolbox
</h1>

<p align="center">
  <strong>A collection of Claude Code plugins that make your AI coding workflow smarter.</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/platform-Claude_Code-blueviolet" alt="Claude Code">
  <img src="https://img.shields.io/badge/version-0.1.0-blue" alt="Version 0.1.0">
</p>

---

Claude Code is great out of the box, but plugins make it better. Claude Toolbox is a curated set of plugins that extend
what Claude Code can do — from discovering your team's coding conventions to automating repetitive workflows.

Each plugin is independently installable, so you only enable what you need.

## Installation

Add the marketplace:

```
/plugin marketplace add milis92/Claude-Toolbox
```

Then pick the plugins you want from the list below.

## Available Plugins

| Plugin                        | Type  | Description                                                            |
|-------------------------------|-------|------------------------------------------------------------------------|
| [create-rule](#create-rule)                       | Skill | Discover and document coding conventions as `.claude/rules/` files     |
| [write-readme](#write-readme)                     | Skill | Create and improve README files with clear structure and real examples |


### `create-rule`

```
/plugin install create-rule@claude-toolbox-marketplace
```

> Rule files (`.claude/rules/<topic>.md`) let you split a large `CLAUDE.md` into smaller, focused files that Claude Code
> loads on demand — only when editing files that match the rule's `paths:` glob. This keeps context lean and relevant.
>
> This skill creates rule files through a guided discovery-to-validation workflow: it samples your real code, identifies
> non-obvious patterns, and produces focused rules — no manual writing needed.

```
- Write a rule file for our testing conventions
- Create a .claude/rules file for error handling patterns
- Document our API endpoint patterns as a rule file
```

<details>
<summary><strong>How it works</strong></summary>

<br>

1. **Discovery** -- Samples 5-10 representative files, identifies project-specific patterns, and presents findings for
   your confirmation.
2. **Write** -- Scans existing rules to avoid duplication, then writes a focused rule file with `paths:` frontmatter,
   `@file/path` references, and `!IMPORTANT` markers.
3. **Eval** -- Launches a subagent in an isolated worktree to verify the rule file actually works. Iterates up to 3
   times if gaps are found.
4. **Completion** -- Recommends reviewing the full setup with `/claude-md-improver`.

</details>

### `write-readme`

```
/plugin install write-readme@claude-toolbox-marketplace
```

> A good README is the front door to your project. This skill creates structured, audience-aware READMEs with centered
> titles, badges, feature lists, installation instructions, usage examples, and configuration tables — whether you're
> starting from scratch or improving what's already there.

```
- Write a README for this project
- Our README is outdated, can you improve it?
- Create documentation for this CLI tool
```

<details>
<summary><strong>How it works</strong></summary>

<br>

1. **Analyze** -- Reads build configs, entry points, source files, and existing docs to understand the project.
2. **Adapt** -- Determines project type (library, CLI, web app, API, internal tool) and tailors the structure.
3. **Write** -- Generates a README with centered title/badges, features, installation, quick start, usage,
   configuration tables, development setup, and architecture overview — separated by horizontal dividers.
4. **Review** -- Presents the draft for your feedback before finalizing.

</details>

## Contributing

Want to add a new plugin? This project has two Claude Code plugins enabled that will guide you through the process.

**plugin-dev** -- Plugin development toolkit

| Skill                             | What it does                  |
|-----------------------------------|-------------------------------|
| `/plugin-dev:skill-development`   | Create a new skill            |
| `/plugin-dev:agent-development`   | Create a new agent            |
| `/plugin-dev:hook-development`    | Create a new hook             |
| `/plugin-dev:command-development` | Create a new slash command    |
| `/plugin-dev:plugin-validator`    | Validate the plugin structure |
| `/plugin-dev:mcp-integration`     | Add MCP server integrations   |

**skill-creator** -- Skill authoring and testing

| Skill                          | What it does                                     |
|--------------------------------|--------------------------------------------------|
| `/skill-creator:skill-creator` | Create, improve, and benchmark skills with evals |

Just invoke the relevant skill and it will walk you through the structure, naming, and best practices.
