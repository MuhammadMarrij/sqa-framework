# 04 — Branching and Git Workflow

**Version:** 1.0
**Author:** Rao Maarij Hassan
**Audience:** All developers

---

## 1. Strategy: Simplified GitHub Flow

We use **GitHub Flow** — not Git Flow.

**Why not Git Flow:** Git Flow was designed for teams that ship versioned releases (e.g., desktop software with v1.2, v1.3). We deploy continuously. Git Flow's `develop`, `release`, and `hotfix` branches add complexity that gives us no value.

**GitHub Flow in one sentence:** Branch off `main`, make changes, open a PR, get it reviewed, merge back to `main`.

---

## 2. Branches

| Branch | Purpose | Lifetime | Protected? |
|---|---|---|---|
| `main` | Always deployable. Source of truth. | Forever | ✅ Yes |
| `feature/*` | New features | Hours to days | ❌ |
| `fix/*` | Bug fixes | Hours | ❌ |
| `chore/*` | Tooling, refactors, dependency updates | Hours | ❌ |
| `docs/*` | Documentation-only changes | Hours | ❌ |

### Branch Naming Convention

```
<type>/<short-description-in-kebab-case>
```

**Examples:**
- `feature/user-login`
- `feature/cart-checkout`
- `fix/cart-total-rounding-bug`
- `chore/upgrade-react-to-19`
- `docs/update-readme`

**Rules:**
- Lowercase only
- Hyphens, not underscores
- Short but descriptive (max ~50 chars)
- One feature/fix per branch

---

## 3. The Lifecycle of a Change

```
  ┌──────────────────────────────────────────────────────┐
  │                                                      │
  │  1. Pull latest main                                 │
  │     $ git checkout main && git pull                  │
  │                                                      │
  │  2. Create branch                                    │
  │     $ git checkout -b feature/user-login             │
  │                                                      │
  │  3. Code + commit                                    │
  │     $ git add . && git commit -m "..."               │
  │                                                      │
  │  4. Push branch                                      │
  │     $ git push -u origin feature/user-login          │
  │                                                      │
  │  5. Open PR on GitHub                                │
  │     Fill out PR template                             │
  │                                                      │
  │  6. CI runs automatically                            │
  │     Wait for green checks                            │
  │                                                      │
  │  7. Request review                                   │
  │     Reviewer uses Code Review Checklist              │
  │                                                      │
  │  8. Address feedback                                 │
  │     Push more commits                                │
  │                                                      │
  │  9. Approved + CI green → merge                      │
  │     Use "Squash and merge"                           │
  │                                                      │
  │ 10. Delete branch                                    │
  │                                                      │
  └──────────────────────────────────────────────────────┘
```

---

## 4. Branch Protection Rules (on `main`)

These rules are configured in GitHub Repository Settings → Branches.

| Rule | Setting | Why |
|---|---|---|
| Require pull request before merging | ✅ ON | No direct commits to `main` |
| Require approvals | ✅ 1 reviewer minimum | Catches what author missed |
| Require status checks to pass | ✅ ON | Code can't merge if CI fails |
| Require branches to be up to date | ✅ ON | Prevents "works on my branch" bugs |
| Require conversation resolution | ✅ ON | All review comments must be addressed |
| Block force pushes | ✅ ON | Prevents history from being rewritten |
| Allow deletions | ❌ OFF | Prevents accidental deletion of `main` |

---

## 5. Commit Message Conventions

Use the **Conventional Commits** standard. It makes history readable and enables automated changelog generation later.

**Format:**
```
<type>: <short description in present tense>

[optional longer body]

[optional footer]
```

**Types:**
- `feat:` — new feature
- `fix:` — bug fix
- `docs:` — documentation only
- `style:` — formatting, no logic change
- `refactor:` — code restructure, no behavior change
- `test:` — adding or updating tests
- `chore:` — tooling, dependencies, config

**Examples:**

✅ Good:
```
feat: add user login endpoint
fix: prevent negative cart totals
test: add coverage for discount edge cases
chore: upgrade vitest to 1.5
```

❌ Bad:
```
update
fixed stuff
asdf
WIP
```

---

## 6. Merge Strategy

We use **Squash and Merge** for all PRs.

**Why:**
- One PR = one commit on `main`
- `main` history stays linear and readable
- Easier to revert (revert one commit, not ten)
- Forces the PR title to be a meaningful commit message

When merging, **the PR title becomes the commit message** — so write good PR titles.

---

## 7. Keeping Your Branch Up to Date

If `main` advances while your PR is open:

```bash
git checkout main
git pull
git checkout feature/your-branch
git rebase main
git push --force-with-lease
```

**Why `--force-with-lease` instead of `--force`:** It refuses to push if someone else has pushed to your branch since you last fetched. A safety net against overwriting a teammate's work.

**Never rebase `main`.** Only rebase your own branches.

---

## 8. What to Do When Things Go Wrong

| Situation | Solution |
|---|---|
| Committed to wrong branch | `git reset --soft HEAD~1`, then checkout correct branch and commit |
| Pushed broken code to your branch | Fix it, commit, push. Don't try to hide it. |
| Merge conflict you don't understand | Ask a teammate. Don't guess and force-push. |
| Accidentally committed secrets/keys | **Tell the team immediately.** Rotate the keys. Don't try to silently `git reset`. |

---

*End of document. See [CI/CD Quality Gates](./05-ci-cd-quality-gates.md) next.*