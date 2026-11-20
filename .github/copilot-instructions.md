# Copilot Instructions

## Project overview

- This repository contains a small FastAPI application for the Mergington High School extracurricular activities website.
- The backend is [src/app.py](/workspaces/skills-getting-started-with-github-copilot/src/app.py).
- The frontend is plain HTML, CSS, and JavaScript under [src/static/](/workspaces/skills-getting-started-with-github-copilot/src/static).
- Activity data is stored only in the in-memory `activities` dictionary in `src/app.py`; restarting the server resets changes.
- The app is launched as the `src.app:app` FastAPI application.

## Setup and commands

- The development container installs dependencies with `pip install -r requirements.txt`.
- Use the VS Code launch configuration named `Launch Mergington WebApp` for the normal development workflow.
- The equivalent command is:

  ```bash
  uvicorn src.app:app --reload --reload-include "src/static/*"
  ```

- The application serves the frontend at `/` and the API documentation at `/docs`.
- Run backend tests from the repository root with `pytest`.
- Put FastAPI tests in a `tests/` directory unless a task explicitly requires another location.

## Backend conventions

- Keep the FastAPI application and route definitions in `src/app.py` unless a feature requires a justified structural refactor.
- Use FastAPI's `HTTPException` for client-facing API errors.
- Use clear, consistent HTTP statuses:
  - `404` when an activity does not exist.
  - `400` for invalid signup or unregister operations.
- Preserve activity names as dictionary keys and student emails as participant strings.
- Treat `max_participants` and the `participants` list as the source of truth for availability.
- Validate unknown activities before reading or mutating activity data.
- Prevent duplicate signups and do not allow a signup when the activity is full.
- When adding an unregister endpoint or related behavior, remove only the matching participant and return a clear `404` or `400` when the request cannot be fulfilled.
- Keep backward-compatible route names and JSON response shapes unless the request explicitly changes the API contract.
- Do not introduce a database or persistence layer for a small feature; in-memory behavior is intentional.

## Frontend conventions

- Keep JavaScript framework-free and use the existing DOM APIs in `src/static/app.js`.
- Use `/activities` and `/activities/{activity_name}/signup` consistently with the existing frontend API calls.
- URL-encode activity names and email values before including them in URLs.
- Use `response.ok` to distinguish successful API responses from errors.
- Show API `detail` messages to the user when available, and retain the existing success/error/hidden message classes.
- Update the activity cards after data changes without requiring a manual page refresh.
- Escape or otherwise safely render user and API-derived text instead of interpolating untrusted content as raw HTML.
- Preserve responsive layout and existing color conventions in `src/static/styles.css`.
- Prefer small, accessible UI changes: semantic elements, descriptive labels, keyboard-operable controls, and visible feedback.

## Testing

- Use `pytest` and FastAPI's TestClient or the project's existing test patterns.
- Follow Arrange-Act-Assert (AAA) structure for tests:
  - Arrange test data or request state.
  - Act through the FastAPI endpoint.
  - Assert status, response body, and resulting activity state.
- Test both success and failure paths for signup and unregister behavior.
- Cover at least:
  - Unknown activity names.
  - Duplicate student signups.
  - Activity capacity limits.
  - Participant removal and missing participant removal.
- Use a fresh copy or fixture for mutable `activities` state so tests do not leak data between cases.
- Add `pytest` to `requirements.txt` if test dependencies are not already declared.

## Implementation workflow

- Read the relevant backend, frontend, and documentation files before editing.
- Make the smallest complete change that satisfies the request.
- Keep backend behavior, frontend rendering, and API documentation in sync.
- For cross-file work, update JavaScript and CSS alongside `src/app.py` and `src/static/index.html` only when needed.
- Run focused tests first, then `pytest` for a full verification when the suite is available.
- Manually verify the browser workflow when changing UI behavior.
- Report exactly what changed and what validation was run.
