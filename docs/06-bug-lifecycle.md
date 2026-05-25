# 06 — Bug Lifecycle and Severity

**Version:** 1.0
**Author:** Rao Maarij Hassan
**Audience:** Whole team

---

## 1. Why This Matters

When bugs are reported inconsistently — sometimes in Slack, sometimes verbally, sometimes never — they get lost. When severity isn't agreed, every bug feels urgent and nothing actually is.

This document defines:
- **How bugs are reported** (the template)
- **How severity is assigned** (P0–P3)
- **How bugs flow** through statuses
- **How fast we respond** to each severity

---

## 2. The Bug Lifecycle

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│   REPORTED  │───▶│   TRIAGED   │───▶│ IN PROGRESS │
└─────────────┘    └─────────────┘    └─────────────┘
                                              │
┌─────────────┐    ┌─────────────┐    ┌──────▼──────┐
│   CLOSED    │◀───│  VERIFIED   │◀───│  IN REVIEW  │
└─────────────┘    └─────────────┘    └─────────────┘
        │
        └────▶ (or REOPENED if bug resurfaces)
```

| Status | Meaning | Who acts |
|---|---|---|
| **Reported** | Bug filed via issue template | Reporter |
| **Triaged** | Severity + owner assigned | SQA / Lead |
| **In Progress** | Developer is fixing | Assignee |
| **In Review** | Fix is in a PR awaiting review | Reviewer |
| **Verified** | Fix is merged and confirmed working | SQA / Reporter |
| **Closed** | Issue is fully resolved | SQA |
| **Reopened** | Bug came back; restart cycle | Anyone |

---

## 3. Severity Levels

Every bug gets a severity. Severity drives priority.

### P0 — Critical (Production Down)
- **What:** Production is broken for most/all users. Data loss possible. Security breach.
- **Examples:** Site won't load. Users can't log in. Payment failures. Exposed credentials.
- **Response time:** Immediately — drop everything.
- **Fix time target:** Within hours.
- **Communication:** Notify whole team. Status update every hour until resolved.

### P1 — High (Major Feature Broken)
- **What:** A major feature is broken, but the app still mostly works.
- **Examples:** Checkout fails for one payment method. Search returns wrong results. Email notifications not sending.
- **Response time:** Same business day.
- **Fix time target:** Within 1–3 days.

### P2 — Medium (Minor Feature Broken or Annoying)
- **What:** A non-critical feature is broken, has a workaround, or affects a small subset of users.
- **Examples:** Date format wrong in one place. Tooltip text missing. Edge-case validation error.
- **Response time:** Within 2 business days.
- **Fix time target:** Within the current or next sprint.

### P3 — Low (Cosmetic or Trivial)
- **What:** Cosmetic issues. Minor inconvenience. "Nice to fix."
- **Examples:** Typo in a label. Slight UI misalignment. Console warning that doesn't affect functionality.
- **Response time:** Within a week.
- **Fix time target:** Whenever there's capacity. Sometimes "won't fix" is the right answer.

---

## 4. How to Decide Severity

Ask three questions:

1. **Who is affected?** All users → higher severity. One user → lower.
2. **Is there a workaround?** No workaround → higher severity. Easy workaround → lower.
3. **What is the impact?** Data loss / money loss / security → P0/P1. Annoyance → P2/P3.

When in doubt, **err on the side of higher severity** at report time. SQA/Lead will downgrade during triage if appropriate.

---

## 5. Bug Reports — The Required Template

Every bug must be reported using the **Bug Report template** (auto-filled when creating a new issue on GitHub).

A good bug report has:
- **Title:** Short and descriptive ("Cart total wrong when discount exceeds 50%")
- **Environment:** Browser, OS, app version
- **Steps to reproduce:** Numbered, reproducible
- **Expected behavior:** What should happen
- **Actual behavior:** What does happen
- **Screenshots/logs:** If available
- **Suggested severity:** Reporter's guess (SQA confirms)

See [`.github/ISSUE_TEMPLATE/bug_report.md`](../.github/ISSUE_TEMPLATE/bug_report.md) for the template.

---

## 6. Triage Process

**Triage runs once a day** (or on-demand for P0/P1).

During triage, SQA (or Lead in SQA's absence):
1. Confirms severity (adjusts if needed)
2. Assigns an owner
3. Adds labels (`area:frontend`, `area:backend`, etc.)
4. Adds to a sprint/milestone if appropriate

**Untriaged bugs older than 1 business day are an SQA failure.** This is the single most important SLA.

---

## 7. Regression Tests Are Mandatory

When a bug is fixed, the PR **must include a test that:**
- Fails before the fix is applied
- Passes after the fix is applied

This prevents the same bug from coming back six months later when someone refactors the area.

A bug-fix PR without a regression test should not be approved.

---

## 8. Bug Metrics We Track

(See `docs/08-quality-metrics.md` for full metrics.)

- **Open bug count** by severity
- **Mean time to triage** (target: < 1 business day)
- **Mean time to resolve** by severity
- **Bug recurrence rate** (bugs reopened within 30 days)

---

*End of document. See [Roles and Responsibilities](./07-roles-and-responsibilities.md) next.*