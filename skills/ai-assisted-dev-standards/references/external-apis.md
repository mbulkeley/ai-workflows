# Third-Party API Integration

Applies when consuming external APIs such as Freshdesk, Jira, GitBook or Stripe.

## Read-only by default (hard rule)

External API calls are read-only (GET, HEAD, OPTIONS) unless the task explicitly requires writes and I have approved them. This holds even when the API supports writes, even when a write would be convenient, and even when the data "should" be updated.

Enforce it in code with a guardrail on every client:

```python
import logging

import httpx

logger = logging.getLogger(__name__)

SAFE_METHODS = {"GET", "HEAD", "OPTIONS"}


class ReadOnlyViolation(Exception):
    """Raised when a mutating HTTP method is attempted without explicit opt-in."""


class ExternalAPIClient:
    """Base client for third-party APIs. Read-only unless allow_writes=True."""

    def __init__(
        self,
        base_url: str,
        *,
        allow_writes: bool = False,
        timeout: float = 10.0,
    ) -> None:
        self.base_url = base_url.rstrip("/")
        self.allow_writes = allow_writes
        self.timeout = timeout
        if allow_writes:
            logger.warning(
                "ExternalAPIClient created with allow_writes=True for %s",
                self.base_url,
            )

    async def request(self, method: str, path: str, **kwargs) -> dict | list | None:
        method = method.upper()
        if method not in SAFE_METHODS and not self.allow_writes:
            raise ReadOnlyViolation(
                f"Blocked {method} {path}: this client is read-only. "
                "Set allow_writes=True only if the write is intended and approved."
            )
        async with httpx.AsyncClient(
            base_url=self.base_url, timeout=self.timeout
        ) as client:
            response = await client.request(method, path, **kwargs)
        response.raise_for_status()
        if response.status_code == 204 or not response.content:
            return None
        return response.json()

    async def get(self, path: str, **kwargs) -> dict | list | None:
        return await self.request("GET", path, **kwargs)
```

Usage:

```python
client = ExternalAPIClient("https://example.freshdesk.com/api/v2")
tickets = await client.get("/tickets")  # allowed
await client.request("POST", "/tickets", json={})  # raises ReadOnlyViolation

# Only when a feature explicitly requires writes, with a comment saying why:
writer = ExternalAPIClient("https://example.freshdesk.com/api/v2", allow_writes=True)
```

Rules around the guardrail:

- Any `allow_writes=True` needs a code comment explaining why the write is needed.
- A blocked write raises `ReadOnlyViolation`. Never catch it just to skip the call silently.
- Tests create clients without `allow_writes` to prove the guardrail is active.
- The example uses Python 3.10+ type syntax (`dict | list | None`).

## Credentials

- One environment variable per service, for example `FRESHDESK_API_KEY` or `JIRA_API_TOKEN`.
- Load settings in one place (for example a `pydantic` settings class in `core/config.py`), not with `os.environ` scattered through service files.
- Never put keys in URLs, logs, error messages or AI prompts.

## Structure

- One service module per integration: `services/freshdesk_service.py`, `services/jira_service.py`.
- Route handlers and MCP tools call the service layer, never the third-party API directly.
- Auth headers are set in one place, inside the client for that integration.

## Rate limits, pagination and retries

- Check the provider's documentation for rate limits before writing an integration, and note the limit and the source in a comment: `# <provider>: <limit> per <period> (source: <docs URL>)`. Don't copy a limit from memory or from another project.
- Never assume a list fits in one response. Always paginate.
- Retry 429 and 5xx responses with exponential backoff and a maximum number of attempts. Respect a `Retry-After` header when present.
- Every call has an explicit timeout.

## Error handling

- Never pass a third-party error straight through to your own callers.
- Translate errors into your own domain errors: a Jira 404 becomes `TicketNotFoundError`.
- Log the third-party response for debugging, minus secrets and personal data.
- Treat 401 and 403 as configuration problems and say so in the error.

## Webhooks

- Validate the webhook signature before processing anything.
- Respond with 200 quickly, then process asynchronously.
- Store the raw payload before processing so a failed run can be replayed.
- Use one endpoint per provider: `/webhooks/freshdesk`, `/webhooks/jira`.

## Testing integrations

- Never call the real API in tests. Mock at the HTTP level, for example with `respx` for `httpx`.
- Test: success, rate limited (429), auth failure (401), malformed response and timeout.
- Keep realistic, anonymized response fixtures in `tests/fixtures/`. No real customer data.
