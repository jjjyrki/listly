id: TASK-0001
status: todo
summary: Scaffold the Vite SPA, NestJS API, and PostgreSQL development setup

# Goal

Create the initial application structure so frontend, backend, and database work can begin safely.

# Problem

Listly needs a consistent local development foundation before feature work starts.

# Scope

In scope:

- Create a Vite SPA using TypeScript.
- Create a NestJS backend using TypeScript.
- Add local PostgreSQL development configuration.
- Add basic environment variable examples for frontend and backend.
- Add scripts for running, linting, testing, and building each app.
- Document local startup steps.

Out of scope:

- Product feature implementation.
- Production deployment infrastructure.
- Authentication, realtime events, and data model details beyond basic connectivity.

# Functional requirements

## 1) Frontend app

- The frontend app starts locally through a documented command.
- The frontend renders a minimal placeholder page.
- The frontend can be configured with the backend API URL.

## 2) Backend app

- The backend starts locally through a documented command.
- The backend exposes a health endpoint.
- The backend can connect to PostgreSQL using environment configuration.

## 3) Database setup

- PostgreSQL can be started locally through documented setup steps.
- Database connection configuration is not hard-coded.
- Example environment files are committed without secrets.

# Acceptance criteria

- [ ] A new developer can start frontend, backend, and PostgreSQL from the README instructions.
- [ ] The frontend placeholder page loads in the browser.
- [ ] The backend health endpoint returns a successful response.
- [ ] Backend startup validates or reports database connectivity.
- [ ] Lint, test, and build commands exist for affected workspaces.

# Testing expectations

- Run frontend lint/build checks.
- Run backend lint/test/build checks.
- Manually verify local startup instructions.

# Risks and mitigations

- Risk: Early structure becomes hard to change.
  - Mitigation: Keep scaffolding minimal and avoid feature-specific abstractions.

# Follow-ups

- Add CI once the initial scripts are stable.
