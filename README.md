<h1 align="center">
Claude Toolbox
</h1>

<p align="center">
  <strong>A Claude Code tooblox with skills, agents, hooks, and commands<br>for working with codebases your way.</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/platform-Claude_Code-blueviolet" alt="Claude Code">
  <img src="https://img.shields.io/badge/version-0.1.0-blue" alt="Version 0.1.0">
</p>

---

Claude Code is powerful out of the box, but it doesn't know your team's conventions -- how you structure tests, handle
errors, name things, or organize code. This plugin extends Claude Code with project-specific workflows so that it works
*with* your patterns, not against them.

## Installation

```bash
claude plugin add /path/to/claude-conventions
```

Or add it to your project's `.claude/settings.json`:

```json
{
  "plugins": [
    "/path/to/claude-conventions"
  ]
}
```

## Skills

<details>
<summary><strong>writing-rule-files</strong> -- Discover and document coding conventions as <code>.claude/rules/</code> files</summary>

<br>

Creates `.claude/rules/<topic>.md` files through a guided discovery-to-validation workflow. Instead of writing
convention docs by hand, this skill samples your real code, identifies non-obvious patterns, and produces focused rule
files that Claude Code respects in every future session.

**Invoke it by asking Claude naturally:**

```
Write a rule file for our testing conventions
Create a .claude/rules file for error handling patterns
Document our API endpoint patterns as a rule file
```

**How it works:**

The skill runs a 4-phase process:

1. **Discovery** -- Clarifies the topic, infers file globs, samples 5-10 representative files from different modules,
   and identifies project-specific patterns. Presents findings for your confirmation before moving on.

2. **Write** -- Scans all existing `.claude/rules/` and `CLAUDE.md` files to avoid duplication, then writes a focused
   rule file with `paths:` frontmatter for contextual loading, `@file/path` references to exemplar code, and`!IMPORTANT`
   markers for critical constraints.

3. **Eval** -- Launches a subagent in an isolated git worktree with a concrete task (write a test, add an endpoint,
   scaffold a screen) and verifies it follows all documented conventions. Iterates up to 3 times if gaps are found.

4. **Completion** -- Recommends reviewing the full setup with `/claude-md-improver`.

**What makes a good rule file:**

| Include                                        | Avoid                                       |
|------------------------------------------------|---------------------------------------------|
| Build/test commands Claude can't guess         | Obvious conventions Claude already knows    |
| Architectural decisions and module boundaries  | Large inline code blocks (use `@file` refs) |
| Code style that differs from language defaults | Tutorials and getting-started material      |
| Repo conventions (commits, branches, PRs)      | Contradictions with existing rules          |
| Environment quirks and gotchas                 | Files over ~200 lines (ideal: 50-80)        |

</details>

## Agents

No agents yet.

## Hooks

No hooks yet.

## Commands

No commands yet.

## Plugin Structure

```
claude-conventions/
  plugin.json            # Plugin manifest
  skills/                # Reusable guided workflows
    writing-rule-files/
      SKILL.md
      evals/
  agents/                # Autonomous task agents
  commands/              # Slash commands
  hooks/                 # Event-driven automation
```

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
