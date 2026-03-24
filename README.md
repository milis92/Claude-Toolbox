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

Claude Code is great out of the box, but plugins make it better. Claude Toolbox is a curated set of skills, agents,
hooks, and commands that extend what Claude Code can do — from discovering your team's coding conventions to automating
repetitive workflows.

## Installation

Add the marketplace and install:

```
/plugin marketplace add milis92/Claude-Toolbox
/plugin install claude-toolbox@claude-toolbox-marketplace
```

## What's Included

| Plugin                        | Type  | Description                                                        |
|-------------------------------|-------|--------------------------------------------------------------------|
| [writing-rule-files](#skills) | Skill | Discover and document coding conventions as `.claude/rules/` files |

## Skills

### `writing-rule-files`

> Creates `.claude/rules/<topic>.md` files through a guided discovery-to-validation workflow. Instead of writing
> convention docs by hand, this skill samples your real code, identifies non-obvious patterns, and produces focused rule
> files that Claude Code respects in every future session.

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

## Contributing

Want to add a new skill, agent, hook, or command? This project has two Claude Code plugins enabled that will guide you
through the process.

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
