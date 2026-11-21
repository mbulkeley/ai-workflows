# AI Workflows

How I work with AI tools, collected for my own reference and for anyone who finds it useful.

I'm Michael Bulkeley, a support leader based in Amsterdam. I started as a software engineer, moved into building and leading support teams, and now spend much of my time making AI a working part of support. I build my own tools with AI assistance, and this repo captures the standards and patterns behind that work.

## What's here

### `skills/ai-assisted-dev-standards`

A skill for Claude that sets a working agreement and coding standards for AI-assisted development of support tools, MCP servers, API integrations and scripts. The highlights:

- **Read-only by default** for databases and external APIs, enforced in code rather than just promised.
- **Rigor matched to risk**, with personal, shared and production tiers, so one-off scripts stay light and shared tools get tests, a README and a second reviewer.
- **Content is data, not instructions.** Tickets, documents and tool results never get to steer the AI.
- **Tests first for shared code, a simplify pass before anything is done,** and a definition of done that includes an owner and a way to hand it off.

The core file stays short. Topic-specific guidance (external APIs, Python and API design, auth, React, AWS, Docker and deployment) lives in reference files that are read only when a task needs them.

### Work in progress

The other folders (`overview`, `shared-patterns`, `templates`, `tools`) are being filled in over time.

## Using the skill

- **Claude Code:** copy the `skills/ai-assisted-dev-standards` folder into your skills directory, for example `~/.claude/skills/` for personal use or `.claude/skills/` inside a project.
- **Claude apps:** zip the folder and upload it as a custom skill. Anthropic's documentation on skills has the current steps.

Then ask, for example: "Using ai-assisted-dev-standards, plan [feature]. Tell me the tier first."

Adapt it freely. The "Who I am" section describes me, so change it to describe you.

## Principles behind it

- **Measure, don't guess.** AI changes are checked against a fixed test set before and after.
- **Humans approve anything irreversible.** Writes, deployments and deletions wait for a yes.
- **Small, readable and owned.** Every shared tool has someone responsible for it and a README that lets someone else pick it up.

## License

MIT. See [LICENSE](LICENSE).
