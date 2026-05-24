id: TASK-0006
status: todo
summary: Build the mobile-first Vite SPA for onboarding, households, lists, items, and history

# Goal

Deliver the first usable Listly web experience for household list sharing.

# Problem

Users need a fast mobile-friendly interface for creating access, switching households, managing lists, and updating shared items.

# Scope

In scope:

- Onboarding routes for create household, join with invite code, and rejoin with rejoin code.
- Household switcher and minimal household controls.
- Profile/settings area for display name, username display, memberships, and leaving a household.
- List overview and list detail views.
- Quick-add item input.
- Inline item editing for title, quantity, and notes.
- Complete, undo-complete, and delete item actions.
- Show completed toggle for recently completed items.
- Read-only list history view.
- Current-list title suggestions while typing.
- Realtime state updates from WebSocket events.
- Mobile-first responsive layout.

Out of scope:

- Email/password authentication UI.
- Notifications.
- Offline mode.
- Deleted-list recovery UI.
- Admin roles or advanced household settings.
- Photos, due dates, colors, icons, and drag-and-drop sorting.

# Functional requirements

## 1) Onboarding

- Users can create a household with username, display name, passcode, and household name.
- Users can join a household with an invite code.
- Users can rejoin an existing membership with rejoin code, username, and passcode.
- Errors are clear and actionable without exposing sensitive details.

## 2) Household and profile

- Users can switch between available households.
- Users can generate or view invite information supported by the backend.
- Users can view the permanent rejoin code where allowed by the product plan.
- Users can change display name if the backend supports it.
- Users can view their permanent username.
- Users can leave a household if the backend supports it.

## 3) Lists and items

- Users can create, rename, and delete lists.
- Users can add items through a single-line quick-add field.
- Users can edit item title, quantity, and notes inline.
- Users can complete and undo-complete items.
- Users can show or hide recently completed items.
- Users can open read-only list history.
- Users can choose supported sort options.

## 4) Realtime and mobile UX

- List changes from other sessions appear without manual refresh.
- Common actions require few taps on mobile.
- Layout is readable in a store-like use case.
- Empty, loading, and error states are handled.

# Acceptance criteria

- [ ] A first-time user can create a household and first list from the SPA.
- [ ] A second user can join with an invite code from the SPA.
- [ ] An existing user can rejoin from the SPA.
- [ ] Users can add, edit, complete, undo-complete, and delete items from a list view.
- [ ] Completed items are hidden by default and visible through a toggle.
- [ ] History is read-only in the UI.
- [ ] Two browser sessions show realtime list updates.
- [ ] The main flows are usable on a phone-sized viewport.

# Testing expectations

- Add component or route tests for onboarding, list detail, and item interactions where existing frontend test tooling supports it.
- Add tests for client-side state updates from realtime events.
- Run frontend lint and build checks.
- Manually verify core flows on a mobile viewport.

# Risks and mitigations

- Risk: MVP UI can grow too broad.
  - Mitigation: Keep settings minimal and defer excluded product features.

# Follow-ups

- Add polished visual design after core flows are usable.
