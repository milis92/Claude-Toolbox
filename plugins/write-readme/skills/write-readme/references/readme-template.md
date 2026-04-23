# README Template

```markdown
<div align="center">

# Project Name

One or two sentences: what this project does and why it exists.
Answer "why should I care?" immediately.

[![CI](https://img.shields.io/github/actions/workflow/status/org/repo/ci.yml?label=CI)](link)
[![Version](https://img.shields.io/github/v/release/org/repo)](link)
[![License](https://img.shields.io/badge/license-MIT-blue)](#license)

[Installation](#installation) · [Quick Start](#quick-start) · [Contributing](#contributing)

</div>

---

![Diagram or hero image](docs/diagram.svg)

---

## Why [Project Name]?

One paragraph: the motivation. What problem does this solve? Why not use existing tools?
This is the most commonly missing section and the most important one for adoption.

> Optional: a callout box with a key constraint or warning.

---

## Highlights

- Feature one — one line, benefit-focused
- Feature two
- Feature three
- Feature four

---

## Installation

**Prerequisites:** list runtime versions, system dependencies.

```sh
# Primary install method
```

Verify the install:
```sh
# Verification command and expected output
```

> Optional tip box for alternative install methods (Docker, package managers, etc.)

---

## Quick Start

The fastest path from zero to "it works." Assume the reader already installed.

```sh
# Minimal working example with expected output
```

---

## Usage

Intro sentence on what this section covers.

### [Workflow or Command Group A]

Description + flags table:

| Flag     | Default | Description  |
|----------|---------|--------------|
| `--flag` | `value` | What it does |

```sh
# Example invocation
```

### [Workflow or Command Group B]

…

---

## Configuration

How settings work across the configuration levels (file, env var, flag).

| Option   | Default | Description      |
|----------|---------|------------------|
| `option` | `value` | What it controls |

```yaml
# Example config file
```

---

## Integrations

How this project integrates with other tools.

### [Integration Name]

```sh
# Setup or usage snippet
```

---

## Contributing

Link to [CONTRIBUTING.md](CONTRIBUTING.md) or brief note on how to open issues/PRs.
Include requirements: tests must pass, code style, etc.

---

## License

[MIT](LICENSE)
```

---

**Notes on adapting this template:**

- Remove any section that doesn't apply — a focused README beats a padded one
- The "Why" section is the most commonly skipped and the most important for adoption
- Navigation links in the header (`[Installation](#installation) · …`) only make sense for longer READMEs
- See `docs/adapting-by-project-type.md` for how to tailor this by project type (CLI, library, API, monorepo)
