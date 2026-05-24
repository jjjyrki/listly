id: TASK-0003
status: todo
summary: Implement lightweight username/passcode auth and household access flows

# Goal

Allow users to create, join, and rejoin households using the lightweight access model from the product plan.

# Problem

Listly's first version does not use email/password accounts, but it still needs secure enough identity and household access controls.

# Scope

In scope:

- Create household and first user in one flow.
- Join household using a valid 7-day invite code.
- Rejoin an existing membership using household rejoin code, username, and passcode.
- Create and validate sessions for authenticated API access.
- Hash and verify passcodes server-side.
- Enforce household membership checks for protected resources.
- Support passcode revalidation after 7 days of inactivity if session infrastructure supports it in this task.

Out of scope:

- Email/password login.
- Passcode recovery.
- Removing other household members.
- Advanced roles or permissions.
- Multi-factor authentication.

# Functional requirements

## 1) Create household

- Accept household name, username, display name, and passcode.
- Reject duplicate usernames.
- Create the user, household, and first membership transactionally.
- Generate a permanent household rejoin code.
- Return an authenticated session.

## 2) Join with invite code

- Accept invite code, username, display name, and passcode.
- Reject expired or invalid invite codes.
- Reject duplicate usernames.
- Create the user and household membership transactionally.
- Mark invite usage if the product flow requires single-use semantics; otherwise preserve multi-use behavior explicitly.
- Return an authenticated session.

## 3) Rejoin household

- Accept rejoin code, username, and passcode.
- Verify the user exists and is already a household member.
- Reject users who are not existing members of that household.
- Restore authenticated access for the membership.

## 4) Authorization

- Protect household, list, and item operations behind membership checks.
- Treat all active household members as equal permission users.

# Acceptance criteria

- [ ] Creating a household creates the first user and membership in one successful operation.
- [ ] Joining with a valid invite creates a new user and membership.
- [ ] Joining with an expired or invalid invite is rejected.
- [ ] Rejoining only works for an existing member with a valid passcode.
- [ ] Raw passcodes are never persisted.
- [ ] Protected endpoints reject users outside the target household.

# Testing expectations

- Add backend tests for create, join, and rejoin success paths.
- Add backend tests for duplicate username, invalid passcode, expired invite, and non-member rejoin failures.
- Add authorization tests for cross-household access denial.

# Risks and mitigations

- Risk: Lightweight passcodes can be weaker than normal passwords.
  - Mitigation: Hash passcodes, rate-limit sensitive flows when available, and avoid exposing whether username or passcode failed.

# Follow-ups

- Add passcode recovery only if the product scope changes.
