# Listly Plan

## Product idea

Listly is a shared shopping list and todo web app for couples, families, roommates, and households. It should feel like a Post-it note on a fridge door: quick to open, easy to update, and visible to everyone who shares the household.

The core experience is simple:

1. A user opens the web app.
2. They choose a shared list, such as `Grocery Store`.
3. They add an item, such as `Milk`.
4. Another household member opens the same list, sees `Milk`, buys it or completes it, and crosses it off.
5. Completed items stay available for 2 days, then disappear from the active list view and remain available in list history.

## Goals

- Make shared household lists fast and simple to use.
- Support multiple people adding and completing items in real time.
- Keep completed items visible briefly so everyone can see what happened.
- Avoid clutter by hiding completed items by default and archiving old completed items.
- Work well on mobile, since most usage will happen while shopping or doing errands.
- Keep the first version product-focused and lightweight.

## Target users

- Couples sharing shopping and household tasks.
- Families coordinating groceries, chores, errands, and reminders.
- Roommates sharing apartment supplies and tasks.

## Core concepts

### User

A user is a lightweight global identity. Users do not need a full email/password account in the first version.

Users have:

- A globally unique username.
- A short alphanumeric passcode.
- A display name that can vary per household.

Username rules:

- Usernames are globally unique across the app.
- Usernames are permanent after creation.
- Usernames are reserved forever, even if the user leaves all households.

Display name rules:

- Display names are for UI display only.
- Display names can be changed anytime.
- Duplicate display names are allowed, but the app should warn when a duplicate is chosen in the same household.

Passcode rules:

- Users choose a short alphanumeric passcode.
- The passcode is required when joining or rejoining on a new device/session.
- The passcode is required again after 7 days of inactivity.
- There is no passcode recovery in the first version. If a user forgets it, they need a new invite and a new username.

### Household

A household is a shared space for a couple, family, roommate group, or similar group. Users can belong to multiple households from the first version.

Household rules:

- All household members have equal permissions.
- Anyone in the household can rename the household.
- There is no explicit household delete action in the first version.
- Members can leave a household voluntarily.
- Members cannot remove other members in the first version.
- If the last member leaves, the household becomes orphaned for 30 days, then is deleted.

### Household membership

A household membership connects a global user to a household.

Membership rules:

- One global user can belong to multiple households using the same username and passcode.
- Display name can be customized per household.
- When joining another household, the app should prefill the current display name, but allow changing it for that household.

### Invite code

An invite code lets a new user join a household.

Invite rules:

- Invite access is lightweight and code-based.
- Invite codes should be human-friendly, such as `BLUE-MILK-42`.
- Invite codes expire after 7 days.
- New household members must use a valid invite code.

### Rejoin code

A rejoin code lets an existing household member restore access.

Rejoin rules:

- Each household has a permanent rejoin code.
- The permanent rejoin code is visible to all existing household members.
- The rejoin code only restores an existing member.
- It does not create new household members.
- Rejoining requires the household rejoin code, username, and passcode.

### List

A list is a named collection of items.

Example lists:

- Grocery Store
- Hardware Store
- Pharmacy
- Chores
- Weekend Tasks
- Packing List

List rules:

- Lists are name-only in the first version.
- List names must be unique within a household.
- There are no list colors, icons, or list types in the first version.
- Every list works the same way.
- Households start with no default lists. Users create their own lists.

### Item

An item is a single thing to buy or do. Items can be active, completed, archived, or soft-deleted.

Item fields:

- Title, for example `Milk`.
- Optional quantity.
- Optional notes, for example `2% lactose-free`.
- Status.
- Created by.
- Created at.
- Completed by.
- Completed at.
- Updated at.

Item rules:

- Duplicate active items are allowed on the same list.
- Anyone in the household can edit, complete, undo-complete, or delete any item.
- Completed items show who completed them and when.
- Completed items are hidden by default behind a `show completed` toggle.
- Completed items remain available in the main list for 2 days.
- After 2 days, completed items are archived and available through the list history.
- Completed item history is retained forever.
- Archived completed items are read-only.
- Archived completed items cannot be restored, but they can be used for title suggestions.
- Manually deleted items are soft-deleted and restorable for 24 hours.

## Required functionality

### 1. Onboarding

The first version should include a dedicated onboarding flow with three main paths:

- Create a household.
- Join with an invite code.
- Rejoin with a rejoin code.

Creating a household should create the first lightweight user at the same time, including:

- Username.
- Display name.
- Passcode.
- Household name.

### 2. Household access

Users can:

- Belong to multiple households.
- Switch between households.
- Generate and share a 7-day invite code.
- View and share the permanent rejoin code.
- Leave a household.
- Rename a household.

The first version should include only minimal household controls, not a broad household settings area.

### 3. Profile/settings

The first version should include a simple profile/settings area where users can:

- Change their display name.
- View their permanent username.
- View household memberships.
- Leave a household.

### 4. Lists

Users can:

- Create lists.
- Rename lists.
- Delete lists.
- View all lists in the selected household.

Deleted list behavior:

- Deleted lists are soft-deleted for 30 days.
- After 30 days, they are permanently deleted.
- The backend should support recovery during the 30-day window.
- The first version does not need a deleted-list recovery UI.

### 5. Adding items

Users can add an item to a selected list.

Add behavior:

- Adding an item requires only a title.
- Item entry should use a single-line quick-add field.
- Quick-add stores exactly what the user typed.
- The app does not parse quantity from quick-add text in the first version.
- Quantity exists as an optional field, but is edited after item creation.
- Notes are optional and edited after item creation.
- Newly added items appear immediately for all household members.

### 6. Editing items

Users can edit item details inline directly in the list.

Editable fields:

- Title.
- Quantity.
- Notes.

### 7. Completing items

Users can cross off an item when it has been bought or completed.

Completion behavior:

- Completed items are visually distinct from active items.
- Completed items show who completed them and when.
- Completed items are hidden by default behind a `show completed` toggle.
- Users can undo completion if they crossed off the wrong item.
- Completed items are archived after 2 days.

### 8. List history

Each list has its own history view.

History behavior:

- Archived completed items are visible in list history.
- History is read-only.
- Completed item history is retained forever.
- History can be used for item title suggestions.

### 9. Suggestions

The first version should include item title suggestions while typing.

Suggestion rules:

- Suggestions come from the current list only.
- Suggestions can include both active and completed items.
- Suggestions are for item titles only.
- Adding from a suggestion copies only the title.
- Matching is case-insensitive.
- Ranking uses a hybrid of recency and frequency.

### 10. Sorting

Lists should support simple sorting options rather than manual drag-and-drop.

Sorting rules:

- Default active item sort is newest first.
- Other sorting options can include oldest first and alphabetical.

### 11. Real-time sync

List updates should be real-time.

When one user adds, edits, completes, undo-completes, or deletes an item, other users viewing the list should see the update without manually refreshing.

The app should handle two users editing the same list at the same time without losing data.

### 12. Mobile-first web experience

The app should work well on phones.

Mobile requirements:

- Common actions should require few taps.
- Lists should be readable while walking through a store.
- The app should support a simple, high-contrast layout.
- The app should require internet access in the first version.

## First version scope

The first version should focus on the fridge-door Post-it experience.

Included:

- Lightweight username/passcode users.
- Multiple households per user.
- Create household and first user together.
- Join household with 7-day invite code.
- Rejoin household with permanent rejoin code, username, and passcode.
- Equal permissions for all household members.
- Simple profile/settings.
- Minimal household controls.
- Household rename.
- Voluntary household leave.
- Orphaned household deletion after 30 days.
- Create, rename, and delete lists.
- List names unique within a household.
- Soft-delete lists for 30 days.
- Add active items through single-line quick-add.
- Inline item editing.
- Optional item quantity.
- Optional item notes.
- Complete and undo-complete items.
- Show completed items through a toggle for 2 days.
- Archive completed items after 2 days.
- Per-list read-only history.
- Keep completed item history forever.
- Current-list title suggestions.
- Simple sorting options, defaulting to newest first.
- Real-time updates.
- Mobile-friendly web UI.

Excluded from first version:

- Email/password accounts.
- Email, SMS, or push notifications.
- Passcode recovery.
- Removing other household members.
- Explicit household deletion.
- Deleted-list recovery UI.
- List categories/types.
- List colors/icons.
- Photos.
- Due dates.
- Offline mode.
- Manual drag-and-drop sorting.
- Advanced permissions.
- Technical architecture details.

## Example user flows

### Create household

1. User opens the app for the first time.
2. User chooses `Create household`.
3. User enters household name.
4. User chooses a globally unique username.
5. User enters a display name.
6. User chooses a passcode.
7. The app creates the household and first member.
8. User creates their first list.

### Invite household member

1. User opens household controls.
2. User generates an invite code.
3. The app creates a human-friendly invite code that expires in 7 days.
4. User shares the code with another person.
5. The invited person opens the app and chooses `Join with invite code`.
6. They choose a globally unique username, display name, and passcode.
7. They join the household.

### Rejoin household

1. Existing member opens the app on a new device or after losing local session access.
2. They choose `Rejoin with rejoin code`.
3. They enter the household rejoin code.
4. They enter their username and passcode.
5. The app restores their existing household membership.

### Add grocery item

1. User opens the app.
2. User selects a household.
3. User selects `Grocery Store`.
4. User types `Milk` in quick-add.
5. User taps `Add`.
6. `Milk` appears as an active item on the list.
7. Other household members see `Milk` immediately.

### Complete grocery item

1. Household member opens the app at the store.
2. They select `Grocery Store`.
3. They see `Milk` as an active item.
4. They buy milk.
5. They tap the checkbox next to `Milk`.
6. `Milk` is marked as completed.
7. `Milk` is hidden by default, but visible through `show completed` for 2 days.
8. After 2 days, `Milk` moves to read-only list history.

### Undo accidental completion

1. User accidentally crosses off `Eggs`.
2. `Eggs` moves to the completed state.
3. User uses undo-complete.
4. `Eggs` returns to the active list.

### Use a suggestion

1. User starts typing `mi` in the quick-add field.
2. The app suggests `Milk` based on active and completed items in the current list.
3. User selects `Milk`.
4. The app fills the title only.
5. User adds the item.

## Data model draft

### User

- `id`
- `username`
- `passcodeHash`
- `createdAt`
- `lastActiveAt`

### Household

- `id`
- `name`
- `rejoinCode`
- `orphanedAt`
- `deleteAfter`
- `createdAt`
- `updatedAt`

### HouseholdMember

- `id`
- `householdId`
- `userId`
- `displayName`
- `joinedAt`
- `leftAt`
- `lastActiveAt`

### InviteCode

- `id`
- `householdId`
- `code`
- `createdByUserId`
- `expiresAt`
- `usedAt`
- `createdAt`

### List

- `id`
- `householdId`
- `name`
- `deletedAt`
- `deleteAfter`
- `createdAt`
- `updatedAt`

### Item

- `id`
- `listId`
- `title`
- `quantity`
- `notes`
- `status`
- `createdByUserId`
- `completedByUserId`
- `createdAt`
- `updatedAt`
- `completedAt`
- `archivedAt`
- `deletedAt`
- `deleteAfter`

## Product decisions

- Completed items are archived after 2 days, not permanently deleted.
- Archived completed items are accessed through per-list history.
- Archived completed items are read-only.
- Completed item history is retained forever.
- All household members have equal permissions.
- Users can belong to multiple households from the first version.
- Access is lightweight invite-code based, not email/password based.
- Invite codes expire after 7 days.
- Existing members can rejoin with a permanent household rejoin code, username, and passcode.
- Usernames are globally unique, permanent, and reserved forever.
- Display names are editable and can vary per household.
- Passcodes are short alphanumeric values, required after 7 days of inactivity.
- There is no passcode recovery in the first version.
- List updates are real-time.
- Notifications are not included in the first version.
- Offline mode is not included in the first version.
- No technical architecture recommendation is included in this plan.
