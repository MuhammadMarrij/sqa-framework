# Code Review Checklist

**Version:** 1.0
**Owner:** SQA (Rao Maarij Hassan)
**Audience:** Anyone reviewing a Pull Request
**Time expected:** 5–20 minutes per PR depending on size

---

## How to Use This Checklist

When you are assigned a PR, work through this checklist top to bottom. You do not have to leave a comment on every item — only on items where something is wrong or unclear.

**You are reviewing for:**
1. **Correctness** — does it actually do what it claims?
2. **Safety** — does it introduce risk (security, data, regressions)?
3. **Clarity** — will the next developer understand this in 6 months?
4. **Standards** — does it follow our framework?

**You are NOT reviewing for:**
- Personal style preferences ("I would have written it differently")
- Re-architecting the whole feature
- Things CI already checked (formatting, lint) — trust the automation

---

## Before You Start Reviewing

- [ ] **CI is green.** If CI is red, do not review yet. Send back to the author.
- [ ] **The PR description is filled out.** If the template is blank, request the author fill it out before review.
- [ ] **The PR is reasonably sized** (< ~400 lines of meaningful change). If it's huge, ask the author to split it.
- [ ] **You understand the context.** Read the linked issue. If you don't understand what's being built, ask before reviewing.

---

## Section 1 — Correctness

- [ ] The change does what the PR description says it does
- [ ] The acceptance criteria from the linked issue are met
- [ ] Edge cases are handled (empty input, null, very large input, network failure, etc.)
- [ ] Error handling is present where it should be (no silent failures)
- [ ] Async code awaits properly — no unhandled promises
- [ ] If a bug is being fixed: a regression test is included that fails without the fix

---

## Section 2 — Tests

- [ ] New business logic has unit tests
- [ ] New API endpoints have integration tests
- [ ] Test names describe behavior in plain English
- [ ] Tests follow the Arrange-Act-Assert pattern
- [ ] No tests are skipped (`.skip`, `xit`, `.only`) without a linked issue explaining why
- [ ] Tests don't depend on each other or on order
- [ ] Tests don't use real user data — fixtures only

---

## Section 3 — Code Quality

- [ ] Names (variables, functions, files) describe what they are/do
- [ ] Functions are short and do one thing
- [ ] No dead code (commented-out blocks, unused imports, unreachable branches)
- [ ] No magic numbers or strings — named constants where it adds clarity
- [ ] Duplication is intentional, not accidental
- [ ] No TODO comments without a linked issue

---

## Section 4 — TypeScript

- [ ] No `any` types unless explicitly justified in a comment
- [ ] No `@ts-ignore` / `@ts-expect-error` without a comment explaining why
- [ ] Function parameters and return types are explicit on public-facing functions
- [ ] Types describe the shape of data accurately (no over-permissive types)

---

## Section 5 — Security & Privacy

- [ ] No secrets, API keys, tokens, or credentials in the code
- [ ] No PII (names, emails, real user data) logged in plaintext
- [ ] User input is validated and sanitized before use
- [ ] Authentication and authorization checks are present where needed
- [ ] No new dependencies with known high/critical vulnerabilities (CI will catch, but spot-check)
- [ ] If the change touches user data: the PR description calls it out

---

## Section 6 — Documentation

- [ ] If behavior changed, README or relevant docs are updated
- [ ] New environment variables are in `.env.example`
- [ ] Public APIs have JSDoc or types describing them
- [ ] Non-obvious code has a comment explaining *why* (not *what*)

---

## Section 7 — Performance (when relevant)

Skip this section if the change is small or clearly not performance-sensitive.

- [ ] No obvious N+1 queries
- [ ] Loops over large datasets are bounded
- [ ] Frontend renders aren't going to cause re-render loops
- [ ] Heavy operations are async / off the main thread where appropriate

---

## Section 8 — Pull Request Hygiene

- [ ] PR title follows Conventional Commits (`feat:`, `fix:`, etc.)
- [ ] PR is linked to its issue
- [ ] Commits are clean (no "WIP", "asdf", "fix typo fix typo fix typo")
- [ ] The branch is up to date with `main`

---

## How to Leave Good Comments

When something is wrong, comment constructively. Use these patterns:

**🛑 BLOCKING — must fix before merge:**
> *"This will throw if `user` is null. Can we add a guard before line 42?"*

**💡 SUGGESTION — non-blocking, take it or leave it:**
> *"`nit:` — could pull this into a helper, but not required."*

**❓ QUESTION — I don't understand:**
> *"Why are we catching this error here instead of letting it bubble up?"*

**👍 PRAISE — call out good work:**
> *"Nice — this is much cleaner than what we had before."*

**Mark each comment with one of `BLOCKING / SUGGESTION / QUESTION / PRAISE`** so the author knows what they must address vs what's optional.

---

## When to Approve

Approve when:
- ✅ All BLOCKING comments are resolved
- ✅ All QUESTION comments are answered
- ✅ CI is green
- ✅ The DoD checklist in the PR is fully checked
- ✅ You would be comfortable owning this code if the author left tomorrow

---

## When to Request Changes

Request changes when:
- ❌ You found a correctness, security, or major design issue
- ❌ Tests are missing where they should exist
- ❌ The PR violates the framework in a way that needs fixing

Be specific. "This doesn't look right" is not feedback — "this will fail if X is empty, here's why" is.

---

## When to Escalate

Escalate to the Lead (Moeez) when:
- You and the author disagree on a substantive issue after one round of discussion
- The PR raises a question the framework doesn't answer (and may need framework updates)
- You don't feel qualified to evaluate part of the change (e.g., security area you don't know)

Escalation is not failure. It is a healthy team behavior.

---

## Final Reminder

**A good review is a careful read, not a rubber stamp.** If you approved a PR in under 90 seconds and it had more than 20 lines of real change, you probably didn't review it.

If the PR is too large or too complex to review properly, **say so** and ask for it to be split. That is part of your job.
