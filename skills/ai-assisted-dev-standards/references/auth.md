# Authentication and Authorization

Applies when a project has user logins or permissions.

## Authentication

- Short-lived access tokens (15 to 60 minutes) with refresh tokens.
- Store tokens in httpOnly, Secure, SameSite=Strict cookies, never in localStorage.
- Hash passwords with bcrypt (cost factor 12 or higher) or argon2. Never MD5 or SHA-1.
- Rate limit or lock out repeated failed logins.

## Authorization

- Keep authentication (who are you?) separate from authorization (what may you do?).
- Use role-based access control or scopes.
- Always check permissions on the backend. Frontend route guards are a convenience, not security.

## General web security

- Validate and sanitize all input, including from authenticated users.
- Use parameterized queries. Never build SQL by joining strings with user input.
- Restrict CORS to specific origins. Never `*` in production.
- Set security headers: `Content-Security-Policy`, `X-Content-Type-Options` and `X-Frame-Options`.
