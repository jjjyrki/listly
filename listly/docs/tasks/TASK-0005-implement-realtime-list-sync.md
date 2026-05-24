id: TASK-0005
status: todo
summary: Implement WebSocket-based real-time sync for household lists and items

# Goal

Keep list state synchronized across household members without manual refresh.

# Problem

Listly's shared-list experience depends on users seeing additions, edits, completions, undo-completions, and deletions as they happen.

# Scope

In scope:

- Add authenticated WebSocket support to the backend.
- Subscribe clients to household-level and list-level rooms.
- Emit normalized events after successful database writes.
- Update frontend state from received events if frontend state infrastructure exists.
- Handle reconnect by refetching authoritative state.
- Document event names and payload shapes.

Out of scope:

- Offline-first sync.
- Conflict-free replicated data types.
- Cursor presence or online-user indicators.
- Cross-region realtime infrastructure.

# Functional requirements

## 1) Connection and authorization

- WebSocket connections are authenticated.
- A connected user can only join rooms for households and lists where they are an active member.
- Unauthorized room joins are rejected.

## 2) Rooms

- Household rooms broadcast household, membership, and list-level changes.
- List rooms broadcast item-level changes.
- Room names or identifiers do not expose sensitive data unnecessarily.

## 3) Events

Emit events after committed database changes for:

- `list.created`
- `list.renamed`
- `list.deleted`
- `item.created`
- `item.updated`
- `item.completed`
- `item.uncompleted`
- `item.deleted`
- `item.archived`

## 4) Client reconciliation

- Clients treat the backend as the source of truth.
- Clients can apply simple event updates to local state.
- Clients refetch current state after reconnect or when an event cannot be applied safely.
- First-version conflict behavior is last-write-wins using backend timestamps.

# Acceptance criteria

- [ ] Two browser sessions viewing the same list both receive item creation events.
- [ ] Item edits, completions, undo-completions, and deletions appear in other active sessions without refresh.
- [ ] List create, rename, and delete events appear for other household members without refresh.
- [ ] Users cannot subscribe to households or lists they do not belong to.
- [ ] Reconnecting clients refetch current state and recover from missed events.
- [ ] Event names and payloads are documented.

# Testing expectations

- Add backend tests for authorized and unauthorized room subscription behavior where practical.
- Add backend tests or integration tests proving events emit after successful mutations.
- Add frontend tests for event reducers/state updates if frontend realtime handling is included.
- Manually verify two-session realtime behavior.

# Risks and mitigations

- Risk: Events can be emitted before a transaction commits.
  - Mitigation: Emit only after successful service operations and database commits.

# Follow-ups

- Add presence indicators only if the product later needs them.
