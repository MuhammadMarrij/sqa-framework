# 07 — Roles and Responsibilities

**Version:** 1.0
**Author:** Rao Maarij Hassan
**Audience:** Whole team

---

## 1. Why Define Roles

In a small team, "everyone is responsible for everything" sounds good but produces "nobody is responsible for anything." Defining roles doesn't mean creating silos — it means making sure each activity has a clear owner.

This document uses a lightweight **RACI** model:

- **R = Responsible** (does the work)
- **A = Accountable** (signs off — only one person per activity)
- **C = Consulted** (provides input)
- **I = Informed** (kept in the loop)

---

## 2. Roles in Our Team

| Role | Who |
|---|---|
| **Developer** | Each engineer on the team |
| **Reviewer** | Any developer reviewing a teammate's PR |
| **SQA** | Rao Maarij Hassan |
| **Lead** | Moeez |
| **Product / Stakeholder** | Whoever defines the requirement |

In a 6-person team, people wear multiple hats. That's fine. What matters is that, *per activity*, the owner is clear.

---

## 3. RACI Matrix — Core QA Activities

| Activity | Developer | Reviewer | SQA | Lead |
|---|---|---|---|---|
| Writing code | **R/A** | — | — | I |
| Writing unit tests | **R/A** | C | C | I |
| Writing integration tests | **R/A** | C | C | I |
| Writing E2E tests | R | C | **A** | I |
| Opening a PR | **R/A** | — | — | I |
| Filling PR template | **R/A** | C | — | — |
| Code review | C | **R/A** | C | I |
| Approving a PR | — | **R/A** | — | C |
| Maintaining CI pipeline | C | — | **R/A** | I |
| Triaging bugs | C | — | **R/A** | C |
| Assigning bug severity | C | — | **R/A** | C |
| Fixing bugs | **R/A** | C | I | I |
| Verifying bug fixes | — | C | **R/A** | I |
| Release readiness sign-off | C | C | **R** | **A** |
| Post-mortem after incident | C | C | **R** | **A** |
| Framework updates | C | C | **R/A** | C |
| Enforcing 100% compliance | — | C | **R** | **A** |

---

## 4. The Developer's Responsibilities

Every developer on the team is responsible for:

- Writing code that meets the **Definition of Done**
- Writing tests alongside their code (not after)
- Self-checking before opening a PR (filling the PR template honestly)
- Responding to review comments within **1 business day**
- Fixing bugs assigned to them within the severity SLA
- Reviewing teammates' PRs when requested
- Ensuring their PR passes 100% of CI gates before requesting review

**The developer owns the quality of their own work.** SQA does not "QA" your PR for you. CI and reviewers verify — but the author is accountable.

---

## 5. The Reviewer's Responsibilities

A reviewer is any developer reviewing a teammate's PR. Reviewers:

- Use the **Code Review Checklist** (see `checklists/code-review-checklist.md`)
- Approve or request changes within **1 business day** of being assigned
- Comment constructively — explain *why*, not just *what*
- Do not approve code they don't understand
- Do not block on personal preferences ("I would have done it differently") — only on substantive issues
- Do not approve PRs that fail any required CI check

**Reviewers verify; they do not rewrite.** If a PR needs major rework, request changes and let the author fix it.

---

## 6. The SQA's Responsibilities

SQA owns the framework and its execution. Specifically:

- Maintains the framework documents in this repo
- Maintains the CI pipeline (`.github/workflows/ci.yml`)
- Triages incoming bugs within **1 business day**
- Verifies bug fixes after merge
- Tracks quality metrics weekly (see `docs/08-quality-metrics.md`)
- Runs the **Release Readiness Checklist** before each release
- Facilitates post-mortems after incidents
- Gathers team feedback and proposes framework updates
- Holds the team accountable to 100% framework compliance

**SQA does not write feature tests for developers.** SQA enables testing infrastructure, defines standards, and verifies they're followed.

---

## 7. The Lead's Responsibilities

The Lead (Moeez):

- Sets technical direction
- Has final say on release sign-off
- Resolves disputes when reviewers and authors disagree
- Approves changes to the framework itself
- Backs SQA when enforcement is challenged
- Ensures the team treats the framework as non-optional

---

## 8. When Roles Conflict

Two common conflicts:

### "The reviewer wants changes the author disagrees with"
- Discuss in PR comments first
- If unresolved within 1 business day, escalate to Lead
- Lead's decision is final

### "A bug is blocking a developer's other work"
- Developer flags to SQA immediately
- SQA re-triages severity and may reassign
- If P0/P1, all hands stop until resolved

---

## 9. What Roles Are NOT

- **SQA is not a gatekeeper who tests every PR manually.** Automation does that.
- **Reviewer is not a co-author.** Their job is to verify, not to write.
- **The Lead is not a tiebreaker for every small decision.** Most things should be resolved between developer and reviewer.

---

*End of document. See [Quality Metrics](./08-quality-metrics.md) next.*