<!--
  Thanks for opening a PR!
  Please fill out every section below. Empty sections mean the PR is not ready for review.
  See checklists/definition-of-done.md and docs/01-framework-overview.md for context.
-->

## 📋 Summary

<!-- One or two sentences describing what this PR does and why. -->



## 🔗 Related Issue

<!-- Link the issue this PR closes. Use the keyword "Closes" to auto-close on merge. -->

Closes #

## 🧩 Type of Change

<!-- Check all that apply. -->

- [ ] ✨ `feat` — new feature
- [ ] 🐛 `fix` — bug fix
- [ ] ♻️ `refactor` — code change that neither adds a feature nor fixes a bug
- [ ] 🧪 `test` — adding or updating tests
- [ ] 📝 `docs` — documentation only
- [ ] 🔧 `chore` — tooling, dependencies, config
- [ ] 🎨 `style` — formatting only, no logic change

## 🛠 What Changed

<!-- A bulleted list of the meaningful changes. Imagine the reviewer has 30 seconds. -->

-
-
-

## 🧪 How I Tested This

<!-- How did you verify the change works? Manual steps, test commands, screenshots. -->



## 📸 Screenshots / Recordings (if UI)

<!-- Drag-and-drop screenshots or screen recordings of the UI change. Delete if not applicable. -->



---

## ✅ Definition of Done

<!-- Every box must be checked (or marked N/A with a reason) before requesting review. -->

### Functional
- [ ] Acceptance criteria from the issue are met
- [ ] Manually tested locally before pushing
- [ ] Edge cases handled (or noted as out of scope)

### Code Quality
- [ ] Follows project conventions
- [ ] No commented-out code, no `console.log`, no debug statements
- [ ] No TODOs without a linked issue
- [ ] No hardcoded secrets or credentials

### Type Safety & Linting
- [ ] `npm run typecheck` passes
- [ ] `npm run lint` passes
- [ ] Prettier formatting applied

### Tests
- [ ] Unit tests added for new business logic
- [ ] Integration tests added for new API endpoints
- [ ] Bug fix? Includes a regression test (fails before fix, passes after)
- [ ] All existing tests still pass
- [ ] Coverage thresholds met

### Documentation
- [ ] Public APIs documented (JSDoc / types)
- [ ] README or relevant docs updated if behavior changed
- [ ] New env variables added to `.env.example`

### Security & Privacy
- [ ] No secrets / API keys / tokens in the code
- [ ] No PII logged in plaintext
- [ ] If this PR touches user data: called out below in "Privacy Notes"
- [ ] No new high/critical vulnerabilities (`npm audit`)

### PR Hygiene
- [ ] PR title follows Conventional Commits (`feat:`, `fix:`, etc.)
- [ ] PR is linked to its issue
- [ ] Branch is up to date with `main`
- [ ] PR is reasonably sized (< ~400 lines of meaningful change)

---

## 🔐 Privacy Notes

<!--
  Does this PR touch personal data, authentication, authorization, or security?
  If yes, describe what and why. If no, write "N/A".
  See docs/10-security-and-compliance.md for what counts as personal data.
-->



## ⚠️ Risk & Rollback

<!--
  What could break? How do we roll back if it does?
  For most PRs this is short. For risky PRs, be specific.
-->

- **Risk level:** Low / Medium / High
- **Rollback plan:**

## 🧠 Notes for Reviewer

<!-- Anything the reviewer should know? Areas of concern? Questions you have? -->



---

<!--
  By opening this PR, you confirm:
  - You have read docs/01-framework-overview.md
  - You have completed the Definition of Done above
  - You have followed the framework — no exceptions without an approved exception issue
-->