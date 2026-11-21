# React Frontend

Applies when a project has a React interface.

## Components

- Functional components with hooks only.
- Keep components small and focused. Split one that passes about 150 lines.
- Keep state in the component that owns it. Don't lift it without a reason.
- Move complex logic into custom hooks such as `useAuth` or `useForm`.

## User experience

- Show a loading state for every async operation.
- Show an error state for every failed request. Never fail silently.
- Disable submit buttons while a request is in flight.
- Use optimistic updates only with a rollback on error.
- Design mobile-first.

## API calls

- Keep all API calls in a service layer such as `services/api.ts`. Components never call `fetch` directly.
- Handle token refresh in one interceptor.
- Cancel stale requests with `AbortController` or React Query.
- Never put secrets or API keys in frontend code. Anything shipped to the browser is public.

## Accessibility (minimum bar)

- Every interactive element works with the keyboard.
- Every image has `alt` text.
- Use semantic HTML (`<button>`, `<nav>`, `<main>`), not a `<div>` for everything.
- Meet WCAG AA color contrast.
