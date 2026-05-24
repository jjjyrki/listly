# Listly Technical Direction

## Summary

Listly should be a Vite single-page app with a backend API, PostgreSQL database, and real-time list synchronization.

Recommended default stack:

- Frontend: Vite SPA with React and TypeScript.
- Backend: NestJS with TypeScript.
- Database: PostgreSQL.
- Real-time transport: WebSockets, likely Socket.IO through NestJS gateways.

Elixir/Phoenix is worth considering because real-time collaboration is a core product requirement, but for this project NestJS is likely the better first choice unless the team specifically wants to optimize around Phoenix Channels and BEAM concurrency from day one.

## Frontend

Use a Vite SPA for the first version.

Suggested choices:

- Vite
- React
- TypeScript
- TanStack Query or a similar server-state library
- A small client-side router, such as TanStack Router or React Router
- CSS modules, Tailwind, or another simple styling approach

The app is mobile-first and online-only in the first version, so the frontend can stay relatively simple:

- Fetch initial household, list, and item state from the API.
- Subscribe to real-time list updates over WebSockets.
- Optimistically update simple interactions where it improves perceived speed.
- Reconcile updates from the backend as the source of truth.

## Backend option A: NestJS

NestJS fits well if we want a conventional TypeScript backend with clear module boundaries.

Strengths:

- Same language across frontend and backend.
- Familiar API structure using controllers, services, modules, guards, and providers.
- Good support for REST APIs and WebSocket gateways.
- Large ecosystem for validation, authentication, testing, job queues, and database tooling.
- Easy to onboard developers who already know Node.js and TypeScript.

Tradeoffs:

- WebSocket support is good, but Phoenix Channels are more mature as a core framework feature.
- Node.js can handle this product's expected real-time workload, but extreme high-concurrency workloads require more care.
- Background jobs, cleanup tasks, and presence tracking need deliberate architecture choices.

Suggested NestJS architecture:

- `AuthModule` for username/passcode sessions.
- `HouseholdsModule` for households, memberships, invites, and rejoin codes.
- `ListsModule` for list CRUD.
- `ItemsModule` for item CRUD, completion, archival, deletion, and suggestions.
- `RealtimeModule` for WebSocket gateway events and room membership.
- `JobsModule` for scheduled archival and cleanup tasks.

## Backend option B: Elixir/Phoenix

Elixir with Phoenix is a strong technical fit for real-time shared-list behavior.

Strengths:

- Phoenix Channels are excellent for real-time updates.
- The BEAM runtime handles many concurrent lightweight connections very well.
- Phoenix Presence can help if we later want online-user indicators or collaborative presence.
- Fault tolerance and supervision are first-class concepts.
- Background processes and scheduled cleanup jobs fit naturally into the platform.

Tradeoffs:

- Adds a second language and ecosystem if the frontend is TypeScript.
- Smaller hiring/onboarding pool than TypeScript/NestJS for many teams.
- More context switching between frontend and backend.
- If the team is not already comfortable with Elixir, early delivery may be slower.

Phoenix is the better choice if real-time scalability, persistent socket connections, presence, and backend reliability are the primary technical bets for the product.

## Database

Use PostgreSQL.

Reasons:

- Strong relational fit for users, households, memberships, lists, items, invite codes, and history.
- Good support for constraints, indexes, transactions, and soft-delete patterns.
- Works well with either NestJS or Phoenix.
- Can support future search and suggestion improvements with indexes or full-text features.

Important database design notes:

- Use UUID or similar stable IDs for externally visible resources.
- Enforce globally unique usernames at the database level.
- Enforce unique active list names within a household.
- Model soft deletion explicitly with `deletedAt` and `deleteAfter` fields.
- Keep completed item history indefinitely.
- Use transactions for multi-step flows like household creation, joining, and rejoining.

## Real-time model

The backend should be the source of truth.

Suggested event flow:

1. Client sends a command through HTTP or WebSocket, such as add item, edit item, complete item, or undo completion.
2. Backend validates permissions and writes the change to PostgreSQL.
3. Backend emits a normalized event to subscribers in the affected household/list room.
4. Clients update local state from the event.

Suggested rooms/topics:

- Household-level room for household, membership, and list changes.
- List-level room for item changes.

Example events:

- `list.created`
- `list.renamed`
- `list.deleted`
- `item.created`
- `item.updated`
- `item.completed`
- `item.uncompleted`
- `item.deleted`
- `item.archived`

For conflict handling, prefer simple last-write-wins behavior for the first version, backed by `updatedAt` timestamps and atomic database writes. Avoid complex collaborative editing unless the product later needs it.

## Recommendation

Start with NestJS unless there is a strong desire to build the backend around Elixir/Phoenix.

For Listly's first version, the real-time needs are important but not unusually complex: users add, edit, complete, and delete list items. NestJS with WebSocket gateways and PostgreSQL should handle this cleanly while keeping the whole app in TypeScript.

Choose Elixir/Phoenix instead if one or more of these are true:

- Real-time infrastructure is the central technical differentiator.
- The team already knows Elixir or wants to invest in it.
- Presence, very high socket concurrency, or long-lived connection reliability are major near-term concerns.
- The project values BEAM fault tolerance more than TypeScript full-stack consistency.

Practical first-version decision:

- Vite SPA + NestJS + PostgreSQL + WebSockets.
- Keep the realtime layer isolated enough that Phoenix could still be reconsidered before the backend becomes large.
