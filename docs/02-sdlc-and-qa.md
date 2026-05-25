# 02 — SDLC and QA Integration

**Version:** 1.0
**Author:** Rao Maarij Hassan
**Audience:** Engineering team, Product, Leadership

---

## 1. What This Document Covers

This document describes how quality activities map to each stage of the **Software Development Life Cycle (SDLC)**.

The core idea: **quality is not just testing.** It is a set of activities performed at every stage of building software. A bug caught during requirements costs almost nothing to fix. The same bug caught after deployment can cost 100x more.

Our framework therefore embeds quality activities into every SDLC stage — not just the testing stage.

---

## 2. The SDLC Model We Use

For a startup of our size, traditional waterfall SDLC does not apply. We use a **lightweight iterative model** with six recognizable stages:

```
┌──────────────┐    ┌──────────────┐    ┌──────────────┐
│ 1. REQUIRE   │───▶│ 2. DESIGN    │───▶│ 3. DEVELOP   │
│   MENTS      │    │              │    │              │
└──────────────┘    └──────────────┘    └──────────────┘
                                                │
┌──────────────┐    ┌──────────────┐    ┌──────▼──────┐
│ 6. MAINTAIN  │◀───│ 5. DEPLOY    │◀───│ 4. TEST     │
│              │    │              │    │              │
└──────────────┘    └──────────────┘    └──────────────┘
        │                                                  
        └─────────────── feedback loop ────────────────▶ back to stage 1
```

Each stage has quality activities, entry criteria (when to start), and exit criteria (when it is done).

---

## 3. Stage-by-Stage Quality Activities

### Stage 1 — Requirements

**Goal:** Understand what we are building and why.

| Aspect | Detail |
|---|---|
| **Entry criteria** | A feature request, bug report, or business need has been raised. |
| **Exit criteria** | A clear, written specification with acceptance criteria. |
| **QA role** | Review requirements for testability and ambiguity. |
| **Key artifact** | GitHub Issue with acceptance criteria. |

**Quality activities:**
- Every feature request must include **acceptance criteria** ("the feature is done when…")
- Ambiguous requirements are flagged before development starts
- Edge cases are listed explicitly (what happens when input is empty, when network fails, when user lacks permission)

**Why this matters:** A vague requirement leads to a vague implementation, which leads to bugs that nobody can confidently call "wrong" because the spec never said what "right" looked like.

---

### Stage 2 — Design

**Goal:** Decide how we will build it.

| Aspect | Detail |
|---|---|
| **Entry criteria** | Requirements are agreed upon. |
| **Exit criteria** | A technical approach is documented (even if just a paragraph in the issue). |
| **QA role** | Identify testing approach and risks early. |
| **Key artifact** | Design notes in the GitHub Issue or a short design doc. |

**Quality activities:**
- For non-trivial features: a brief technical approach is written in the issue
- Test strategy is discussed: *what kind of tests will prove this works?*
- Risks are identified: *what could break? what depends on this?*

**Why this matters:** Thinking about testing during design forces you to write testable code. Code that is hard to test is usually code that is hard to maintain.

---

### Stage 3 — Development

**Goal:** Write the code.

| Aspect | Detail |
|---|---|
| **Entry criteria** | Design is agreed upon. Feature branch is created. |
| **Exit criteria** | Code is written, tests are written, all CI passes locally. |
| **QA role** | Enforce standards via automation (lint, types). |
| **Key artifact** | Commits on a feature branch. |

**Quality activities:**
- Code follows ESLint and Prettier rules (enforced on save)
- TypeScript strict mode catches type errors at write-time
- Tests are written **alongside** the code, not after
- Developer runs tests locally before pushing

**Why this matters:** The cheapest place to catch a bug is in the editor while you are writing the code. Static analysis tools do this for free.

---

### Stage 4 — Testing

**Goal:** Verify the code does what it should.

| Aspect | Detail |
|---|---|
| **Entry criteria** | Code is pushed and PR is opened. |
| **Exit criteria** | All automated tests pass, manual review approves. |
| **QA role** | Define what tests are required, maintain CI pipeline. |
| **Key artifact** | Green CI checks on the PR. |

**Quality activities:**
- Automated CI runs unit, integration, and end-to-end tests
- Code coverage is reported
- Security scan runs on dependencies
- A human reviewer applies the Code Review Checklist

**Why this matters:** Tests in a PR catch regressions before they reach users. Tests in production catch them after users have already been affected.

---

### Stage 5 — Deployment

**Goal:** Ship the code to users.

| Aspect | Detail |
|---|---|
| **Entry criteria** | PR is approved and merged to `main`. |
| **Exit criteria** | Code is running in production and verified. |
| **QA role** | Define release readiness criteria, smoke-test post-deploy. |
| **Key artifact** | A deployed release. |

**Quality activities:**
- The **Release Readiness Checklist** is verified before deployment (see `checklists/release-readiness.md`)
- Critical user flows are smoke-tested manually after deployment
- Rollback plan exists in case something breaks

**Why this matters:** A merge is not a deployment. A deployment is not a verified release. Each step needs its own check.

---

### Stage 6 — Maintenance

**Goal:** Keep the code healthy and respond to issues.

| Aspect | Detail |
|---|---|
| **Entry criteria** | Code is running in production. |
| **Exit criteria** | Continuous — never truly "done." |
| **QA role** | Track bugs, prioritize fixes, identify patterns. |
| **Key artifact** | Bug reports, the bug tracker, post-mortems. |

**Quality activities:**
- Bugs are reported using the Bug Report Template
- Bugs are triaged with severity (P0–P3) within 1 business day
- Recurring bugs trigger a root-cause discussion
- Dependencies are reviewed monthly for security updates

**Why this matters:** Most software lives in maintenance longer than it spent in development. Quality in maintenance keeps the codebase from rotting.

---

## 4. The "Shift Left" Principle

You will hear this term in QA. It means: **catch defects as early in the SDLC as possible.**

| Where bug is caught | Relative cost to fix |
|---|---|
| Requirements | 1x |
| Design | 3x |
| Development | 10x |
| Testing | 30x |
| Production | 100x+ |

*(Numbers are illustrative — the principle, not the exact ratio, is what matters.)*

Our framework "shifts left" by:
- Requiring acceptance criteria at the requirements stage
- Encouraging design discussion before coding
- Automating checks in the editor and on every commit
- Catching defects before they reach `main`

---

## 5. Entry and Exit Criteria — Summary Table

| Stage | Entry Criteria | Exit Criteria |
|---|---|---|
| 1. Requirements | Need raised | Spec + acceptance criteria written |
| 2. Design | Spec agreed | Technical approach documented |
| 3. Development | Design agreed | Code + tests written, lint passes |
| 4. Testing | PR opened | CI green, reviewer approved |
| 5. Deployment | PR merged | Code live, smoke test passed |
| 6. Maintenance | Code live | (continuous) |

---

## 6. Roles per Stage

| Stage | Primary Owner | Supporting |
|---|---|---|
| Requirements | Product / Lead | SQA (reviews testability) |
| Design | Developer | SQA (reviews test approach) |
| Development | Developer | — |
| Testing | Developer (writes tests) + Reviewer | SQA (maintains CI) |
| Deployment | Developer / Lead | SQA (verifies checklist) |
| Maintenance | Whole team | SQA (tracks bug metrics) |

---

*End of document. See [Testing Strategy](./03-testing-strategy.md) next.*