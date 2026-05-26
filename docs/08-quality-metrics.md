# 08 — Quality Metrics

**Version:** 1.0
**Author:** Rao Maarij Hassan
**Audience:** SQA, Lead, whole team

---

## 1. Why Measure

A framework that nobody measures is a framework that nobody follows. Metrics tell us:
- Is the framework actually working?
- Where are we slow?
- What needs to change in v2?

Metrics are **diagnostic, not punitive.** They are used to improve the process, not to evaluate individuals.

---

## 2. Principles for Choosing Metrics

We pick metrics that are:
1. **Easy to collect** (no manual data entry)
2. **Actionable** (a bad number suggests a clear next step)
3. **Honest** (hard to game without doing the right thing)
4. **Few** (5 good metrics beat 20 noisy ones)

---

## 3. The v1 Metric Set

We track six metrics. SQA reviews them weekly.

### 3.1 PR Cycle Time
- **What:** Time from PR opened to PR merged
- **Target:** Median under 2 business days
- **Why:** Long cycle times indicate review bottlenecks or PRs that are too large
- **Source:** GitHub PR data

### 3.2 CI Pass Rate (First Attempt)
- **What:** % of PRs where CI passes on first push
- **Target:** > 70%
- **Why:** Low pass rate means developers aren't running checks locally before pushing
- **Source:** GitHub Actions logs

### 3.3 Open Bug Count by Severity
- **What:** Count of open bugs, grouped P0/P1/P2/P3
- **Target:**
  - P0: 0 open
  - P1: ≤ 2 open
  - P2: ≤ 10 open
  - P3: untargeted (acceptable backlog)
- **Why:** Critical/high bugs should not accumulate
- **Source:** GitHub Issues with severity labels

### 3.4 Mean Time to Triage (MTTT)
- **What:** Average time from bug reported → severity assigned + owner set
- **Target:** < 1 business day
- **Why:** Untriaged bugs are invisible bugs
- **Source:** GitHub Issues timestamps

### 3.5 Mean Time to Resolve (MTTR) by Severity
- **What:** Average time from bug triaged → bug verified fixed
- **Target:**
  - P0: < 1 day
  - P1: < 3 days
  - P2: < 2 weeks
  - P3: untargeted
- **Why:** Each severity has a different urgency; we should meet our own SLAs
- **Source:** GitHub Issues timestamps

### 3.6 Bug Recurrence Rate
- **What:** % of closed bugs reopened within 30 days
- **Target:** < 10%
- **Why:** High recurrence means regression tests aren't catching what they should
- **Source:** GitHub Issues — closed then reopened

---

## 4. What We Deliberately Do NOT Track in v1

These metrics sound useful but cause more harm than good early on:

| Metric | Why we skip it |
|---|---|
| Lines of code | Rewards verbose code; punishes refactors |
| Number of commits per developer | Rewards busy work, not value |
| Code coverage as a ranking | Coverage is a quality signal, not a leaderboard |
| Number of bugs filed per developer | Discourages reporting, which is the worst possible outcome |
| Time spent in code review | Encourages rubber-stamping |

The principle: **don't measure what's easy if it changes behavior in the wrong direction.**

---

## 5. How Metrics Are Reported

### Weekly
SQA posts a short summary in the team channel every Monday:

```