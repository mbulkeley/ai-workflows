# Python, API Design and Logging

## Python style

- Follow PEP 8, formatted with Black (88-character lines).
- Type hints on all function signatures: `def get_user(user_id: int) -> User:`
- Google-style docstrings on public functions and classes.
- Prefer `dataclasses` or `pydantic` models over raw dicts for structured data.
- Use `pathlib` rather than `os.path`, and `httpx` for HTTP (it supports async and timeouts).

## Project structure

- Separate concerns: routes or tools, then a service layer, then a data layer. No business logic in route handlers.
- Pass dependencies in explicitly (FastAPI `Depends` or function parameters). No hidden global state.
- Keep files under about 300 lines. Split a file when it grows.
- Group by feature, not by type: `users/router.py` and `users/service.py`, not `routers/users.py`.

## Error handling

- Never use a bare `except:`. Catch specific exceptions.
- Use custom exception classes for domain errors, for example `UserNotFoundError`.
- Return structured errors: `{"error": "message", "code": "ERROR_CODE"}`.
- Log the full traceback for unexpected errors.

## Dependencies

- Pin versions in `requirements.txt` or `pyproject.toml`.
- Always use a virtual environment.
- Keep development dependencies separate from production ones.
- Check a package is well maintained before adding it.

## REST API design

- Nouns for resources, not verbs: `/users/{id}`, not `/getUser`.
- Correct methods: GET reads, POST creates, PUT or PATCH updates, DELETE deletes.
- Consistent response shapes across endpoints.
- Status codes: 422 validation error, 401 not authenticated, 403 not authorized, 404 not found.
- Version the API: `/api/v1/...`
- Validate every request with Pydantic schemas.
- Never expose internal IDs, stack traces or database errors in responses.
- Paginate any list that could return more than about 20 items.
- Document every endpoint (FastAPI's automatic docs are fine).
- Rate limit authentication endpoints at minimum. Use idempotency keys for mutations that might be retried.

## Logging

What to log:

- Every request: method, path, status code and duration (use middleware).
- Authentication events: login, logout, failed attempts, token refresh.
- Unhandled exceptions with the full traceback.
- Key business events, without personal data.

What never to log:

- Passwords, tokens, API keys or any other secret.
- Full request bodies by default, since they may contain personal data.
- Debug noise in production.

Format:

- Structured JSON logs in production: `{"level": "info", "event": "user_login", "user_id": 42}`.
- Use the `logging` module, never `print()`.
- Levels: DEBUG (development only), INFO (normal operations), WARNING (unexpected but handled), ERROR (needs attention), CRITICAL (system failure).
