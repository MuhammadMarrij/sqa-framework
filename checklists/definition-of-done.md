# Definition of Done (DoD)

**Version:** 1.0
**Owner:** SQA (Rao Maarij Hassan)
**Audience:** All Developers

---

## What is the Definition of Done?

The Definition of Done (DoD) is a simple checklist of quality criteria that **every single Pull Request (PR)** must meet before it can be merged into the `main` branch. 

These rules ensure we maintain high code quality, robust security, and reliable automated testing as a team.

---

## The DoD Checklist

### 1. Functional Correctness
- [ ] The code implements the features or fixes the bug described in the linked issue.
- [ ] All acceptance criteria defined in the user story are fully met.
- [ ] The change has been smoke-tested manually in a local development environment.
- [ ] Common edge cases (empty input, null values, network timeouts, etc.) are handled or documented.

### 2. Code Quality & Standards
- [ ] Follows the team's coding guidelines and conventions.
- [ ] No commented-out code, debug statements, or `console.log` statements remain.
- [ ] No `TODO` comments are left in the codebase without a linked GitHub issue tracking them.
- [ ] No hardcoded secrets, API keys, or credentials.

### 3. Type Safety & Formatting
- [ ] ESLint passes without errors (`npm run lint`).
- [ ] Strict type checking passes (`npm run typecheck`).
- [ ] Prettier formatting has been applied.

### 4. Automated Tests
- [ ] Unit tests are written for all new business logic (using Jest).
- [ ] Integration tests are written for all new or modified API endpoints (using Supertest).
- [ ] Bug fixes include a regression test that fails without the fix and passes with it.
- [ ] All existing tests continue to pass.
- [ ] Code coverage thresholds are met (60% backend, 50% frontend).

### 5. Documentation
- [ ] Public APIs, modules, and complex functions are documented (JSDoc or types).
- [ ] Any user-facing configuration or setup changes are added to the project README.
- [ ] Any new environment variables are documented with placeholder values in `.env.example`.

### 6. Security & Privacy
- [ ] All inputs are validated and sanitized.
- [ ] Authentication and authorization checks are applied to all new endpoints.
- [ ] No Personally Identifiable Information (PII) is logged in plaintext.
- [ ] Dependency check shows no new high or critical vulnerabilities (`npm audit`).

### 7. Pull Request Hygiene
- [ ] The PR title follows Conventional Commits format (e.g., `feat: add email verification`, `fix: correct user password validation`).
- [ ] The PR is linked to its tracking issue.
- [ ] Git commit history is clean (no temporary or chaotic commits).
- [ ] The branch is up-to-date with `main`.
- [ ] The PR is reasonably sized (< ~400 lines of meaningful code changes).

---

## Verification

Before requesting code review, the PR author must tick all the boxes in the PR description template confirming compliance. Reviewers are responsible for verifying these items during code review.
