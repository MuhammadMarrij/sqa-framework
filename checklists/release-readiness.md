# Release Readiness Checklist

**Version:** 1.0
**Owner:** SQA (Rao Maarij Hassan)
**Audience:** Lead, SQA, deploying developer
**Run by:** SQA before every release. Lead signs off.

---

## When to Use This

Use this checklist **before every deployment to production**, regardless of whether it's a feature release, a bug fix, or a hotfix.

A release is **not ready** until every applicable item below is checked. If something doesn't apply (e.g., docs-only change), mark it `N/A` with a brief note.

---

## Release Metadata

Fill in before starting the checklist:

- **Release name / version:** _________
- **Deploying developer:** _________
- **Reviewed by SQA:** _________
- **Signed off by Lead:** _________
- **Deployment target:** _________ (staging / production)
- **Planned deploy time:** _________
- **Rollback plan link:** _________

---

## Section 1 — Code Readiness

- [ ] All planned PRs for this release are merged to `main`
- [ ] `main` is green (all CI checks passing on the latest commit)
- [ ] No P0 or P1 bugs are open against features in this release
- [ ] All commits in this release follow Conventional Commits
- [ ] The release branch (if used) is up to date with `main`

---

## Section 2 — Testing Verification

- [ ] All unit tests pass on `main`
- [ ] All integration tests pass on `main`
- [ ] E2E test suite has been run against staging — **all critical paths green**
- [ ] Manual smoke test of the top user flows on staging:
  - [ ] Login / signup
  - [ ] Core happy path (e.g., main feature flow)
  - [ ] Any new feature included in this release
- [ ] Regression tests for any bug fix in this release have been run

---

## Section 3 — Build & Artifacts

- [ ] The build completes successfully (`npm run build` clean)
- [ ] No new warnings in the build output that weren't there last release
- [ ] Bundle size has not unexpectedly grown (check vs. last release if available)
- [ ] Static assets are in place (images, fonts, etc.)

---

## Section 4 — Environment & Config

- [ ] All required environment variables are documented in `.env.example`
- [ ] Production environment has all required variables set
- [ ] No `.env` or secrets accidentally bundled into the build
- [ ] Database migration plan (if applicable):
  - [ ] Migrations tested on staging
  - [ ] Migrations are reversible (or rollback path is documented)
  - [ ] Migrations will run before app code expects new schema
- [ ] Third-party services (analytics, monitoring, payment, email) configured for production

---

## Section 5 — Security

- [ ] No new High or Critical vulnerabilities from `npm audit`
- [ ] No secrets or credentials in the codebase or build output
- [ ] If new endpoints added: authentication and authorization verified
- [ ] If new PII fields added: handling reviewed per `docs/10-security-and-compliance.md`
- [ ] HTTPS / TLS enforced on all production endpoints

---

## Section 6 — Documentation

- [ ] README updated if user-facing setup changed
- [ ] Changelog / release notes drafted (what's new, what's fixed)
- [ ] API documentation updated if endpoints changed
- [ ] Any new feature flags documented (default value, when to flip)

---

## Section 7 — Monitoring & Observability

- [ ] Error tracking is wired up (e.g., Sentry or equivalent) and verified working in staging
- [ ] Key metrics / dashboards exist for any new feature
- [ ] Alerts are configured for any new critical path
- [ ] Logs from new code are appropriately leveled (no debug logs in production)

---

## Section 8 — Rollback Plan

- [ ] Rollback procedure is documented for this release
- [ ] Previous version is tagged in Git and recoverable
- [ ] If database migrations are included: downgrade procedure is documented
- [ ] Whoever is on-call knows where the rollback steps live
- [ ] Estimated rollback time is known (target: < 15 minutes)

---

## Section 9 — Communication

- [ ] Team notified of the deployment window
- [ ] Stakeholders informed of new features / breaking changes
- [ ] If user-facing changes: customer-facing comms drafted (email, in-app banner, changelog)
- [ ] On-call developer identified for the post-deploy window

---

## Section 10 — Deployment Window

Confirm before pressing the deploy button:

- [ ] Not deploying on Friday afternoon or before a holiday (unless emergency)
- [ ] Not deploying during peak user traffic (if known)
- [ ] At least one other team member is available in case of issues
- [ ] You (the deployer) can stay available for at least 1 hour post-deploy

---

## Sign-Off

This release is approved for deployment when both signatures are present:

- **SQA Approval:** _________ Date: _________
- **Lead Approval:** _________ Date: _________

---

## Post-Deployment Checklist (run immediately after deploy)

- [ ] Application loads in production
- [ ] Login / signup works
- [ ] No spike in error rate (check monitoring dashboard for 15 min)
- [ ] No spike in latency
- [ ] Key user flows smoke-tested in production
- [ ] If issues found: rollback decision within 30 minutes, do not "wait and see"

---

## Post-Deployment Notes

Record any anomalies, observations, or lessons learned:

_________________________________________________
_________________________________________________
_________________________________________________

These notes feed into the framework retrospective.

---

## Why This Checklist Exists

Deployments are where confident teams ship and overconfident teams break things. The checklist is **not bureaucracy** — it is the difference between "I think it's ready" and "I know it's ready."

A 10-minute checklist prevents the 4-hour incident.