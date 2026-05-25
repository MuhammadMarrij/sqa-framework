# 03 — Testing Strategy

**Version:** 1.0
**Author:** Rao Maarij Hassan
**Audience:** All developers

---

## 1. Purpose

This document defines **what** we test, **how** we test it, and **what tools** we use.

The goal is not 100% coverage. The goal is **confidence** — when we ship code, we want to be confident it works and confident a regression will be caught before it reaches users.

---

## 2. The Testing Pyramid

We follow the classic testing pyramid: **many fast tests, fewer slow tests.**

```
                  ▲
                 ╱ ╲
                ╱E2E╲           ← 5–10 critical user journeys
               ╱─────╲             Slow, expensive, fragile
              ╱       ╲            Run on PR + nightly
             ╱Integrat.╲         ← API endpoints, DB integration
            ╱───────────╲           Medium speed, medium cost
           ╱             ╲          Run on every PR
          ╱    Unit       ╲       ← Functions, components, utilities
         ╱─────────────────╲         Fast, cheap, stable
        ╱___________________╲        Run on every save, every PR
```

### Target Distribution

| Layer | % of total tests | Why |
|---|---|---|
| **Unit** | ~70% | Cheap, fast, catch most bugs |
| **Integration** | ~20% | Catch bugs at module boundaries |
| **E2E** | ~10% | Catch bugs only visible end-to-end |

**Why a pyramid:** Unit tests run in milliseconds. E2E tests run in minutes. A team with an inverted pyramid (mostly E2E) ends up with a CI pipeline so slow that developers stop running it. Slow tests get skipped. Skipped tests catch nothing.

---

## 3. Tooling Choices

We pick **one tool per layer**. More tools = more configuration, more onboarding, more places for things to break.

| Layer | Tool | Why |
|---|---|---|
| **Frontend Unit** | Vitest + React Testing Library | Native Vite/TS support, fast, modern, identical API to Jest |
| **Backend Unit** | Vitest | Same tool both sides = less to learn |
| **Backend Integration** | Vitest + Supertest | Supertest is the industry standard for testing Express APIs |
| **E2E** | Playwright | Modern, reliable, supports multiple browsers, great debugging |
| **Linting** | ESLint (with TypeScript plugin) | Standard |
| **Formatting** | Prettier | Standard |
| **Type-checking** | TypeScript compiler (`tsc --noEmit`) | Built-in |

**Why Vitest over Jest:** Vitest is built for Vite/TypeScript projects, runs faster, and has a near-identical API. If your team knows Jest, they already know Vitest.

**Why Playwright over Cypress:** Better TypeScript support, faster, supports multiple browsers in one config, and has built-in trace viewer for debugging flaky tests.

---

## 4. What to Test — by Layer

### 4.1 Unit Tests

**What to test:**
- Pure functions (utilities, formatters, validators)
- Business logic in isolation
- React components — render, props, user interactions
- Custom hooks
- Helper functions

**What NOT to test:**
- Third-party libraries (trust them — they have their own tests)
- Trivial getters/setters
- Configuration objects

**Example:**
A function `calculateCartTotal(items, discount)` is a perfect unit test target. You give it inputs, check outputs. No database. No network.

---

### 4.2 Integration Tests

**What to test:**
- API endpoints (request → response, including DB interaction)
- Database queries (against a real test database)
- Service-to-service interactions
- Authentication and authorization flows

**What NOT to test:**
- The same logic already covered in unit tests
- Browser behavior (that's E2E)

**Example:**
`POST /api/users` should accept a valid payload, save to DB, return 201 with the new user. Reject invalid payloads with 400. Reject duplicate emails with 409. These are integration tests.

---

### 4.3 End-to-End (E2E) Tests

**What to test:**
- The **critical user journeys** — the 5–10 things users do most often
- Login / signup
- Core purchase / submission flow
- Anything where a regression would be catastrophic

**What NOT to test:**
- Every page
- Every form
- Edge cases (those belong in unit/integration)

**Example:**
"User signs up, adds an item to cart, checks out, sees confirmation." That's one E2E test. If it passes, the major plumbing of your app works.

**Why be selective:** E2E tests are slow (seconds to minutes each) and **flaky** (network blips, animation timing, etc.). A flaky E2E test is worse than no test — developers learn to ignore failures.

---

## 5. Code Coverage Targets

Coverage is a **proxy** for test quality, not a measure of it. You can have 100% coverage with garbage tests, or 50% coverage with excellent tests.

That said, we set realistic targets to encourage discipline.

### v1 Targets *(Aspirational — not enforced until Phase 3)*

| Code area | Target | Enforcement |
|---|---|---|
| **Backend business logic** | 60% line coverage | Warning in Phase 1, blocking in Phase 3 |
| **Backend API endpoints** | 70% line coverage | Warning in Phase 1, blocking in Phase 3 |
| **Frontend utilities/hooks** | 60% line coverage | Warning in Phase 1, blocking in Phase 3 |
| **Frontend components** | No target initially | We'll set one after Phase 2 |
| **E2E coverage** | 5–10 critical paths | Manually tracked |

### Why These Numbers

- **60–70% is honest.** Most healthy codebases sit in this range. Teams that claim 90%+ usually have either trivial tests or a dedicated QA team writing them full-time.
- **Components have no target initially** because component tests have a high effort-to-value ratio early on. We'll revisit.
- **Targets warn, not block, at first.** Hard gates on coverage from day one cause developers to write low-quality tests just to hit the number. We want a culture of writing meaningful tests first.

---

## 6. When to Write Tests

| Situation | Test required? |
|---|---|
| Adding new business logic | ✅ Yes — unit tests |
| Adding a new API endpoint | ✅ Yes — integration tests |
| Adding a critical user flow | ✅ Yes — E2E |
| Fixing a bug | ✅ Yes — a regression test that proves the bug is fixed |
| Refactoring (no behavior change) | ✅ Existing tests should still pass |
| Changing CSS / pure styling | ❌ Optional |
| Updating docs | ❌ No |
| Renaming variables | ❌ No |

**The bug-fix rule is critical:** Every bug fix MUST include a test that fails before the fix and passes after. This prevents the same bug from coming back.

---

## 7. Test Naming and Structure

Tests are read more often than they are written. We write them to be understood.

**Use the `describe` / `it` pattern:**

```typescript
describe('calculateCartTotal', () => {
  it('returns 0 when cart is empty', () => { ... });
  it('sums item prices correctly', () => { ... });
  it('applies discount as a percentage', () => { ... });
  it('throws if discount exceeds 100%', () => { ... });
});
```

**Naming rule:** The test name, read in plain English, should describe the behavior. If you can't name it clearly, you probably don't understand what you're testing.

**Use AAA structure:**

```typescript
it('sums item prices correctly', () => {
  // Arrange
  const items = [{ price: 10 }, { price: 20 }];

  // Act
  const total = calculateCartTotal(items, 0);

  // Assert
  expect(total).toBe(30);
});
```

---

## 8. Test Data and Fixtures

- **Never use real user data in tests.** Use fixtures.
- **Each test should be independent.** No test should depend on another test running first.
- **Reset state between tests.** Clear the database, reset mocks, in `beforeEach`.

---

## 9. Sample Tests

See [`examples/sample-backend-test.ts`](../examples/sample-backend-test.ts) and [`examples/sample-frontend-test.tsx`](../examples/sample-frontend-test.tsx) for working examples that follow the patterns in this document.

---

*End of document. See [Branching and Git Workflow](./04-branching-and-git.md) next.*