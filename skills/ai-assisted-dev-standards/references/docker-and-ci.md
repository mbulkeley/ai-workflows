# Docker, Database Migrations and Deployment

Applies when a project uses containers, a schema-managed database or a deployment pipeline.

## Docker

- Run as a non-root user.
- Install dependencies before copying application code, so the dependency layer stays cached.
- Never put secrets in a `Dockerfile` or `docker-compose.yml`. Use `env_file` locally and a secrets manager in production.
- Local development should start with one command: `docker compose up`.
- Use multi-stage builds to keep images small.
- Add a `.dockerignore` that excludes at least `.venv/`, `node_modules/`, `.env`, `.git/` and `__pycache__/`.

## Database migrations

- Every model change gets a migration, committed in the same change as the model.
- Always read a generated migration before applying it. Autogenerate tools miss renames and some constraints.
- Never edit a migration that has already run in production.
- Migrations against shared or production databases need explicit approval, like any other write.

## Deployment

Pick the approach that matches the project. Both are acceptable when done with discipline.

**With CI/CD** (for example GitHub Actions or Bitbucket Pipelines):

- Every push runs: secret scan, lint, tests, build. Any failure blocks the merge.
- Deploy only from the main branch after CI passes.
- Authenticate the pipeline to the cloud with short-lived credentials (OIDC) rather than stored keys.
- Deploy to staging first, then production after approval or passing smoke tests.

**Manual deployment** (common for small shared tools):

- Keep a short written runbook: the exact steps, how to verify success and how to roll back.
- Deploy only after the tests pass locally and the change has been reviewed.
- Deploy at a time when you can watch the result, never just before leaving for the day.
- Note what was deployed and when, for example in the commit message, a changelog or a team channel.

Never deploy code with failing tests, and never skip a required migration step.
