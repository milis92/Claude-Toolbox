# Adapting the Template by Project Type

The README template is a starting point. Adapt it based on what the project is and who it's for.

## Libraries / Packages

**Emphasize:** Installation, Usage (with API examples), API Reference.

- Quick Start: show `npm install` / `pip install` / `cargo add` and a minimal working snippet
- Usage: show primary API surface with realistic inputs and outputs
- API Reference: link to generated docs (TypeDoc, pdoc, rustdoc) if they exist
- Skip: Architecture, Contributing (unless open-source)

## CLI Tools

**Emphasize:** Quick Start (install + one command), Usage (common commands), Configuration.

- Quick Start: one install command, one working invocation, expected output
- Usage: command reference table (`| Command | Description | Flags |`)
- Configuration: env vars or config file table
- Consider adding a "Why" section explaining motivation over alternatives

## Web Applications / Internal Tools

**Emphasize:** Architecture, Configuration, Development setup.

- Lead with an Architecture table: `| Layer | Tech | Port |`
- Include API endpoints table if the app exposes routes
- Add a scripts table: `| Command | Description |`
- Configuration: environment variables table with Required column
- Getting Started: docker-compose or equivalent one-liner
- Development: split frontend/backend setup if applicable
- For internal tools: include what systems this integrates with and who to contact

## APIs / Services

**Emphasize:** Quick Start (how to call the API), Authentication, endpoints.

- Quick Start: show a curl or SDK example with real response
- Authentication: how to get credentials, where to pass them
- Endpoints: table with Method, Path, Description
- Link to full OpenAPI spec or hosted docs

## Monorepos

**Emphasize:** Project map near the top.

- Add a packages/apps table right after the intro: `| Package | Description | Docs |`
- Each sub-project should have its own README
- Root README focuses on the big picture and how the pieces fit together

## Open-Source Projects

Additional sections to include:

- **Contributing**: link to CONTRIBUTING.md, or brief note on how to open issues/PRs
- **License**: always present, with a link to the LICENSE file
- **Authors / Maintainers**: GitHub handles or contributors page link
- **Support**: where to get help (GitHub Issues, Discord, etc.)
- **Changelog**: link to CHANGELOG.md or releases page

## Internal / Private Projects

Skip or simplify:

- License (usually not applicable)
- Authors (team name and contact channel is enough)
- Contributing (contribution process is implicit)

Add instead:

- Who owns this and which Slack channel to use
- What internal systems it integrates with
- Links to internal wikis or Confluence docs
