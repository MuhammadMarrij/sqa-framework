# 05 — CI/CD Quality Gates

**Version:** 1.0
**Author:** Rao Maarij Hassan
**Audience:** All developers + team lead

---

## 1. What Are Quality Gates?

A **quality gate** is an automated check that runs on every Pull Request. If it fails, the PR cannot merge.

Quality gates make our process **enforceable without effort**. A developer doesn't have to remember to run the linter — CI runs it for them. A reviewer doesn't have to check formatting — Prettier already did.

---

## 2. The Gates (in order they run)

```
PR opened or updated
        │
        ▼
┌─────────────────────┐
│ GATE 1: Install     │  Cache deps, install fresh
└─────────────────────┘
        │
        ▼
┌─────────────────────┐
│ GATE 2: Lint        │  ESLint — code style + bug patterns
└─────────────────────┘
        │
        ▼
┌─────────────────────┐
│ GATE 3: Type Check  │  TypeScript — `tsc --noEmit`
└─────────────────────┘
        │
        ▼
┌─────────────────────┐
│ GATE 4: Unit Tests  │  Vitest — fast, run first
└─────────────────────┘
        │
        ▼
┌─────────────────────┐
│ GATE 5: Integration │  Supertest — API tests
└─────────────────────┘
        │
        ▼
┌─────────────────────┐
│ GATE 6: Build       │  Verify code actually compiles
└─────────────────────┘
        │
        ▼
┌─────────────────────┐
│ GATE 7: Security    │  npm audit + dependency scan
└─────────────────────┘
        │
        ▼
┌─────────────────────┐
│ GATE 8: Coverage    │  Report (warning only in v1)
└─────────────────────┘
        │
        ▼
   All green → ready for review
```

---

## 3. Gate-by-Gate Detail

### Gate 1 — Install Dependencies
- **What it does:** Installs `node_modules`. Uses npm cache to speed this up.
- **Blocks merge?** Yes (if install fails, nothing else works).
- **Typical time:** 20–60 seconds (cached).

### Gate 2 — Lint (ESLint)
- **What it does:** Checks code style and common bug patterns.
- **Blocks merge?** ✅ Yes — any error blocks.
- **Why blocking:** Lint errors are cheap to fix and catch real bugs (e.g., unused variables, missing dependencies in React hooks).

### Gate 3 — Type Check (TypeScript)
- **What it does:** Runs `tsc --noEmit` to catch type errors.
- **Blocks merge?** ✅ Yes.
- **Why blocking:** A type error is almost always a bug. If TypeScript says no, listen.

### Gate 4 — Unit Tests (Vitest)
- **What it does:** Runs all unit tests.
- **Blocks merge?** ✅ Yes — any failing test blocks.
- **Typical time:** Seconds.

### Gate 5 — Integration Tests (Supertest)
- **What it does:** Spins up the API, runs endpoint tests against a test database.
- **Blocks merge?** ✅ Yes.
- **Typical time:** 30 seconds – 2 minutes.

### Gate 6 — Build
- **What it does:** Compiles the project (`npm run build`).
- **Blocks merge?** ✅ Yes — if it doesn't build, it can't ship.

### Gate 7 — Security Scan
- **What it does:** Runs `npm audit` and checks dependencies for known vulnerabilities.
- **Blocks merge?** ⚠️ Partial — blocks only on **Critical** or **High** severity. Warns on Medium/Low.
- **Why partial:** Blocking on every low-severity issue would halt all work whenever a transitive dependency gets a CVE.

### Gate 8 — Coverage Report
- **What it does:** Generates code coverage report. Posts as a comment on the PR.
- **Blocks merge?** ❌ No — **warning only in Phase 1 and 2**. Becomes blocking in Phase 3.
- **Why not blocking yet:** Forcing coverage from day one produces fake tests written to game the metric.

---

## 4. Test Pyramid in CI

To keep CI fast, we run tests in the order they're cheap:

1. Lint + types first (< 1 minute) — fail fast
2. Unit tests next (< 2 minutes)
3. Integration tests after (< 5 minutes)
4. E2E tests (Playwright) run on a **separate workflow**, not on every PR.

**Why split E2E off:** E2E tests are slow (10+ minutes) and flaky. We run them:
- On merge to `main` (post-merge verification)
- Nightly against staging
- Manually triggered before releases

This keeps PR feedback fast without sacrificing E2E coverage.

---

## 5. Caching Strategy

CI is slow if it reinstalls dependencies every run. We cache:
- `node_modules` (keyed on `package-lock.json` hash)
- Vitest cache
- TypeScript build cache

This reduces a typical CI run from ~8 minutes to ~3 minutes.

---

## 6. Required Status Checks on `main`

The following checks are **required to pass** before merging:

- ✅ `lint`
- ✅ `typecheck`
- ✅ `test-unit`
- ✅ `test-integration`
- ✅ `build`
- ✅ `security-scan` (critical/high only)

The following are **reported but not required**:

- ⚠️ `coverage` (Phase 1–2)
- ⚠️ `test-e2e` (runs on separate workflow)

---

## 7. When Tests Are Flaky

A flaky test is one that sometimes passes and sometimes fails without code changes.

**Rules:**
1. **Never** merge with a known flaky test as "we'll fix it later." It WILL be ignored.
2. If a test fails intermittently, **fix it or delete it within 24 hours**.
3. A flaky test makes the whole pipeline untrustworthy. One flaky test poisons everything.

---

## 8. Implementation File

The actual workflow file lives at `.github/workflows/ci.yml`. See it for the executable definition of these gates.

A human-readable explanation is also in `workflows/ci-pipeline-explained.md`.

---

*End of document. See [Bug Lifecycle and Severity](./06-bug-lifecycle.md) next.*