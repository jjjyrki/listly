id: TASK-0004
status: todo
summary: Implement list, item, completion, deletion, archive, and history APIs

# Goal

Provide the backend API needed for the core shared-list product experience.

# Problem

Users need to create lists, add and edit items, complete or undo items, hide old completed items, and view read-only history.

# Scope

In scope:

- List create, rename, soft-delete, and list retrieval.
- Item create, edit, complete, undo-complete, and soft-delete.
- Active list view with optional completed item visibility for recently completed items.
- Read-only list history for archived completed items.
- Backend support for archiving completed items after 2 days.
- Backend support for permanent deletion of orphaned households and soft-deleted lists according to product rules if scheduled jobs are available.
- Current-list title suggestions using active and completed item titles.

Out of scope:

- Frontend UI implementation.
- Realtime event delivery.
- Deleted-list recovery UI.
- Offline mode.
- Manual drag-and-drop sorting.

# Functional requirements

## 1) Lists

- Users can create lists in a household where they are an active member.
- List names are unique among active lists in the same household.
- Users can rename lists.
- Users can soft-delete lists for 30 days.
- Deleted lists are excluded from normal list views.

## 2) Items

- Users can add items with a required title and optional quantity and notes.
- Quick-add behavior stores exactly the title provided by the caller.
- Users can edit title, quantity, and notes.
- Duplicate active item titles are allowed.
- Users can soft-delete items with a 24-hour restore window supported by backend state.

## 3) Completion and archive

- Users can complete and undo-complete items.
- Completed items store who completed them and when.
- Completed items remain available for 2 days.
- Completed items older than 2 days become archived and read-only.
- Archived completed items remain available in list history indefinitely.

## 4) Sorting and suggestions

- Default active item sort is newest first.
- Support oldest first and alphabetical sorting if included in the endpoint contract.
- Suggestions are scoped to the current list.
- Suggestions match case-insensitively and return titles only.
- Suggestion ranking uses a simple recency/frequency approach.

# Acceptance criteria

- [ ] Members can create, rename, and soft-delete lists in their household.
- [ ] Members can add, edit, complete, undo-complete, and soft-delete items.
- [ ] Non-members cannot access or mutate household lists or items.
- [ ] Recently completed items can be included in the active list response when requested.
- [ ] Archived completed items are available through a read-only history endpoint.
- [ ] Archived completed items cannot be edited, completed, uncompleted, or deleted through normal item mutation endpoints.
- [ ] Suggestions are list-scoped and case-insensitive.

# Testing expectations

- Add backend tests for list CRUD and unique active names.
- Add backend tests for item lifecycle transitions.
- Add backend tests for archive/read-only behavior.
- Add backend tests for sorting and suggestion behavior.
- Add authorization tests for cross-household access denial.

# Risks and mitigations

- Risk: Item status transitions can become inconsistent.
  - Mitigation: Centralize transition logic in a service and test each allowed and rejected transition.

# Follow-ups

- Add deleted-list recovery UI in a later task if product scope expands.
