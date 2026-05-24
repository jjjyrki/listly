id: TASK-0002
status: todo
summary: Create the core PostgreSQL schema for users, households, lists, and items

# Goal

Implement the first database schema for Listly's core product model.

# Problem

The app needs durable relational storage with constraints that protect household, list, and item behavior.

# Scope

In scope:

- Add migrations for users, households, household members, invite codes, lists, and items.
- Add database-level uniqueness and foreign key constraints.
- Add timestamps and soft-delete fields required by the product plan.
- Add indexes for common lookups.
- Add a seed or fixture path only if already aligned with the backend tooling.

Out of scope:

- API endpoint implementation.
- Realtime event delivery.
- Advanced search or suggestion ranking beyond schema support.
- Production data migration from an existing system.

# Functional requirements

## 1) Users

- Store globally unique permanent usernames.
- Store passcode hashes, not raw passcodes.
- Track creation and last active timestamps.

## 2) Households and memberships

- Store households with name, permanent rejoin code, orphaning fields, and timestamps.
- Store household memberships with per-household display name, join/leave state, and last active timestamp.
- Allow one user to belong to multiple households.

## 3) Invites

- Store invite codes with household, creator, expiry, used state, and creation timestamp.
- Support lookup by invite code.

## 4) Lists and items

- Store lists scoped to households.
- Enforce unique active list names within a household.
- Store items with title, optional quantity, optional notes, status, creator, completion metadata, archive metadata, delete metadata, and timestamps.

# Acceptance criteria

- [ ] Migrations create all core tables from a clean database.
- [ ] Migrations can be rolled back according to the chosen migration tooling.
- [ ] Usernames are unique at the database level.
- [ ] Active list names are unique within a household at the database level.
- [ ] Foreign keys prevent orphaned child records where appropriate.
- [ ] Common household, list, item, invite, and username lookups are indexed.

# Testing expectations

- Run migration tests or equivalent schema checks.
- Add tests for key constraints if the repository has a database test pattern.
- Manually verify migrate/reset behavior against local PostgreSQL.

# Risks and mitigations

- Risk: Soft-delete uniqueness can be implemented incorrectly.
  - Mitigation: Use explicit partial unique indexes where supported by PostgreSQL.

# Follow-ups

- Add schema support for richer suggestions if MVP ranking needs more than item history.
