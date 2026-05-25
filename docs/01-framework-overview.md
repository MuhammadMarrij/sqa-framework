# 01 — QA Framework Overview

**Version:** 1.0
**Author:** Rao Maarij Hassan
**Last Updated:** May 2026
**Audience:** Entire engineering team

---

## 1. Purpose

This document defines our team's **Software Quality Assurance Framework** — the standards, processes, and automated checks that ensure code we ship is reliable, maintainable, and reviewed.

It exists for three reasons:

1. **We have no existing process.** A small team without standards develops inconsistent habits that compound over time.
2. **We will scale.** A framework adopted at 6 people is invisible. A framework adopted at 20 people is impossible.
3. **Quality is a shared responsibility.** Without clear standards, "quality" means whatever the loudest person in the room thinks it means today.

---

## 2. Scope

### In Scope for v1
- All TypeScript code (frontend and backend)
- Pull request workflow
- Automated CI checks
- Code review standards
- Definition of Done
- Bug reporting and triage

### Out of Scope for v1 *(Future Phases)*
- Performance and load testing
- Production monitoring and alerting
- Accessibility audits
- Manual exploratory test cases
- Mobile testing
- Penetration testing

---

## 3. Guiding Principles

The framework rests on five principles. When in doubt, these win over specific rules.

### 3.1 Automate before you mandate
A rule enforced by a machine is followed 100% of the time. A rule written in a wiki is followed when convenient. We invest in automation first.

### 3.2 Block what is cheap to fix; warn on what is expensive
Linting errors block merges — they take seconds to fix. Code coverage below target produces a warning, not a block — chasing arbitrary numbers wastes time. We calibrate strictness to cost.

### 3.3 The author owns quality
The developer who wrote the code is responsible for its quality. Reviewers verify and catch what the author missed. Reviewers are not a substitute for the author doing their job.

### 3.4 Tests are documentation
A well-written test tells the next developer what the code is supposed to do. We write tests as if a teammate will read them in six months — because they will.

### 3.5 This is v1
This framework will be wrong in places. We will discover those places. We update the framework when we do. Process is a product — it has versions and iterations.

---

## 4. Framework Architecture — Four Layers of Quality

Quality is enforced at four layers. Each layer catches what the previous layer missed.

```
┌────────────────────────────────────────────────────────────┐
│  LAYER 4 — HUMAN REVIEW                                    │
│  A peer reviewer checks logic, design, and edge cases.     │
│  Tools: GitHub PR review + Code Review Checklist           │
├────────────────────────────────────────────────────────────┤
│  LAYER 3 — AUTOMATED TESTS                                 │
│  Unit, integration, and end-to-end tests run on every PR. │
│  Tools: Vitest, Supertest, Playwright                      │
├────────────────────────────────────────────────────────────┤
│  LAYER 2 — STATIC ANALYSIS                                 │
│  Code is type-checked, linted, and formatted automatically.│
│  Tools: TypeScript, ESLint, Prettier                       │
├────────────────────────────────────────────────────────────┤
│  LAYER 1 — AUTHOR SELF-CHECK                               │
│  Developer verifies their own work before opening PR.      │
│  Tools: PR template, Definition of Done                    │
└────────────────────────────────────────────────────────────┘
```

**Why layered:** Catching a defect at Layer 1 costs seconds. At Layer 4, minutes. In production, hours or days. Each layer is cheaper than the next.

---

## 5. The Pull Request as the Unit of Quality

Every change to our codebase flows through a **Pull Request (PR)**. A PR is where all four quality layers come together.

A PR is **mergeable** when:

1. ✅ The author has self-checked against the PR template
2. ✅ All CI checks pass (Layers 2 and 3)
3. ✅ At least one reviewer has approved (Layer 4)
4. ✅ The Definition of Done is met
5. ✅ The branch is up to date with `main`

No exceptions for "small changes" or "urgent fixes." Small changes are the most common source of bugs precisely because they bypass scrutiny.

---

## 6. What Developers Will Actually Do

In daily practice, this framework asks each developer to:

| When | What |
|---|---|
| **Before writing code** | Pull latest `main`, create a feature branch |
| **While writing code** | Write tests alongside code, run lint locally |
| **Before opening a PR** | Self-check the Definition of Done |
| **When opening a PR** | Fill out the PR template honestly |
| **During review** | Respond to comments within 1 business day |
| **As a reviewer** | Use the Code Review Checklist, approve or request changes within 1 business day |

That's the entire daily ask. Everything else in this framework supports these six actions.

---

## 7. What This Framework Is *Not*

To prevent misunderstanding, this framework is **not**:

- ❌ A policing tool used to evaluate developers individually
- ❌ A substitute for engineering judgment
- ❌ A complete process — we will add to it as we learn
- ❌ A copy of how Google or Meta works (they have different constraints)
- ❌ Mandatory in its entirety from day one (see rollout plan)

---

## 8. Related Documents

| Document | Purpose |
|---|---|
| [02 — SDLC and QA Integration](./02-sdlc-and-qa.md) | How quality activities map to each stage of development |
| [03 — Testing Strategy](./03-testing-strategy.md) | What to test, how to test, tools and coverage targets |
| [04 — Branching and Git Workflow](./04-branching-and-git.md) | How we use Git as a team |
| [05 — CI/CD Quality Gates](./05-ci-cd-quality-gates.md) | What checks run on every PR |
| [06 — Bug Lifecycle and Severity](./06-bug-lifecycle.md) | How bugs are reported, triaged, and resolved |
| [07 — Roles and Responsibilities](./07-roles-and-responsibilities.md) | Who owns what in the QA process |
| [08 — Quality Metrics](./08-quality-metrics.md) | How we measure whether the framework is working |
| [09 — Rollout Plan](./09-rollout-plan.md) | The phased plan for adopting this framework |

---

## 9. Open Questions

These are intentionally unresolved in v1 — they require team input:

- **Coverage thresholds:** Initial targets are proposals (see doc 03). Actual numbers should be calibrated after the team has written tests for one sprint.
- **Reviewer assignment:** Random assignment, round-robin, or by domain expertise? To be decided with the team.
- **Time-to-review SLA:** Is one business day realistic for our team's workload? To be validated.
- **E2E test ownership:** Who writes them — the feature author or a dedicated QA pass? Open.

---

*End of document. See [SDLC and QA Integration](./02-sdlc-and-qa.md) next.*