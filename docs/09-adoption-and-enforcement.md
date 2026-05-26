# 09 — Adoption and Enforcement

**Version:** 1.0
**Author:** Rao Maarij Hassan
**Audience:** Whole team, Lead

---

## 1. Our Approach: Full Enforcement from Day One

This framework is **adopted in full from the day it is approved**. There is no phased rollout, no "warning-only" period, and no opt-outs.

Per direction from the Lead:

> Every developer must pass this framework. Code that does not meet the framework cannot be committed or merged.

This document explains how full enforcement works in practice and what to expect during the transition.

---

## 2. What "100% Enforcement" Means

Concretely:

| Activity | Enforced How |
|---|---|
| Code style + formatting | ESLint + Prettier — runs locally on save, blocks in CI |
| Type safety | TypeScript strict mode — blocks in CI |
| Unit tests | Jest — must pass, blocks in CI |
| Integration tests | Jest + Supertest — must pass, blocks in CI |
| Coverage thresholds | Jest coverage config — blocks in CI when below target |
| Pre-commit checks | Husky — runs lint + type-check before commit is allowed |
| PR template | Required on every PR |
| Code review | At least one reviewer approval required — enforced via branch protection |
| Definition of Done | Required checklist in every PR — must be all-checked |
| Branch protection | Direct commits to `main` blocked — enforced by GitHub |

**There is no merge button if anything is red.** That is the rule.

---

## 3. Approval and Effective Date

| Event | Date |
|---|---|
| Framework v1 proposal delivered | _[fill in delivery date]_ |
| Team review / feedback window | _[fill in — 2-3 days suggested]_ |
| Framework approved by Lead | _[fill in]_ |
| Effective date — enforcement begins | _[fill in]_ |

Once the effective date is reached, **all new commits and PRs must comply**. There is no grace period for code written after that date.

Code that existed *before* the effective date is grandfathered — we don't retroactively fail existing files. But any PR that touches an existing file must bring that file into compliance.

---

## 4. The First Week — What to Expect

Honesty: full day-one enforcement will feel slow at first. Developers who haven't used the tools before will hit friction. **This is expected and temporary.**

Common first-week friction points:
- ESLint will flag dozens of style issues per file → run `npm run lint --fix` and most auto-fix
- TypeScript strict mode may surface real type errors → fix them; they were latent bugs
- Husky will reject commits that fail lint/types → fix the issue and recommit
- Coverage may be below threshold for new files → write tests; that's the point

**SQA's role in week one:** Pair with any developer who is stuck. Friction is not failure — friction is the framework working.

---

## 5. How a New Developer Onboards

When a new dev joins:

1. **Day 1:** Read `README.md` and `docs/01-framework-overview.md`
2. **Day 1:** Clone repo. Run `npm install`. Verify `npm run lint`, `npm test`, `npm run typecheck` all execute.
3. **Day 1:** Make a tiny dummy PR (e.g., fix a typo in a doc) to feel the full PR + CI flow end-to-end.
4. **Day 2:** Read the testing strategy, DoD, and code review checklist.
5. **Day 2 onward:** Ship real work, follow the framework.

Target: **a new developer can merge their first PR within 2 working days.**

---

## 6. Handling Pushback

Some pushback is expected. Common objections and how SQA responds:

| Objection | Response |
|---|---|
| "This slows me down." | Yes, in the short term. It speeds up the team's whole velocity by catching bugs early. We are optimizing for the team, not for any single PR. |
| "My change is too small to need a test." | Small changes are the most common source of bugs. The framework applies to all changes. |
| "The coverage threshold is unrealistic." | Take it to a framework retrospective. Targets can change — but we don't lower them mid-stream to make individual PRs pass. |
| "Can we skip the review for this hotfix?" | No. Hotfixes need *faster* review, not skipped review. P0 fixes get reviewed within the hour. |
| "I disagree with the style rule." | Open an issue proposing the change. Until changed, the rule stands. |

The framework is enforceable because the Lead has committed to backing it. Disputes escalate to the Lead.

---

## 7. Exceptions Process

Genuine exceptions exist — for example, a third-party file that can't pass our lint rules, or a one-time migration script that doesn't need full test coverage.

To request an exception:

1. Open an issue with label `framework-exception`
2. Describe what rule you want exempted and why
3. SQA and Lead review within 1 business day
4. If approved: the exception is documented in the relevant config (e.g., `.eslintignore`)
5. If denied: the developer brings the code into compliance

**Verbal exceptions don't exist.** Everything is written down so we know what's exempt and why.

---

## 8. Reviewing the Framework Itself

The framework is a living document. We review it on a regular cadence.

| Review | When | Who |
|---|---|---|
| Quick check-in | End of each sprint | SQA |
| Framework retrospective | Once a month | Whole team |
| Major version update (v2) | When patterns of pain emerge | SQA proposes, Lead approves |

In retrospectives we ask:
1. Which rules are catching real bugs?
2. Which rules cause friction without preventing bugs?
3. What new risk is the framework not covering?

Rules can be **removed**, not just added. A shrinking framework is a sign of maturity.

---

## 9. What Success Looks Like

After the framework has been in effect for one month, success means:

- Every merged PR has passing CI, an approval, and a filled DoD
- Bug triage happens within 1 business day
- Developers stop asking "do I need to write a test for this?" — they just do
- New developers onboard and merge their first PR within 2 working days
- The team trusts that `main` is always deployable

If those things are true, the framework is working. If not, we adjust in v2.

---

## 10. Final Note from the SQA

This framework is the **starting line, not the finish line.** Day one enforcement is rigorous because we are building a culture from scratch and a strong culture is easier to enforce than to introduce later.

The framework will get smarter over time. It will get *less* heavy in some places (rules we never needed) and *more* targeted in others (real risks we discover). Your feedback is what makes that happen.

— Rao Maarij Hassan, SQA

---

*End of document. End of `docs/` series. Continue to [`checklists/`](../checklists/) for the operational checklists.*