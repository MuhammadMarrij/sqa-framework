# 10 — Security and Compliance

**Version:** 1.0
**Author:** Rao Maarij Hassan
**Audience:** Whole team, Lead, Stakeholders

---

## 1. Important Disclaimer

This document establishes **engineering practices** that are aligned with data protection and security expectations across jurisdictions our team and users may operate in (Pakistan, United Kingdom, United States, European Union).

**This is not a legal compliance certification.** Formal compliance with regulations such as GDPR, UK DPA 2018, PDPB (Pakistan), CCPA, HIPAA, SOC 2, or PCI-DSS requires:
- Independent legal review
- For some standards, third-party audits
- Documented organizational policies beyond engineering

No legal review has been conducted for this framework. Before processing real user data in production — especially across borders — the team should obtain qualified legal counsel.

---

## 2. Why Engineers Care About Compliance

You may think "compliance is HR/legal's problem." It is not.

Most data protection laws place the **technical implementation burden directly on engineers**:
- How is data stored?
- Who can access it?
- How is it deleted when a user requests it?
- How are breaches detected and disclosed?

Bad engineering creates legal exposure. Good engineering reduces it. This framework focuses on the engineering side.

---

## 3. Regulations That May Apply

Depending on where the company operates and where users are located, the following may apply:

| Region | Regulation | What it primarily covers |
|---|---|---|
| **Pakistan** | Personal Data Protection Bill (PDPB) | Personal data of Pakistani residents (pending finalization at time of writing) |
| **UK** | UK GDPR + Data Protection Act 2018 | Personal data of UK residents |
| **EU** | GDPR | Personal data of EU/EEA residents |
| **US (federal)** | Various sector-specific (HIPAA for health, COPPA for minors, GLBA for financial, SOX for public companies) | Sector-dependent |
| **US (state)** | CCPA/CPRA (California), VCDPA (Virginia), CPA (Colorado), and growing | Personal data of residents of those states |
| **Global** | PCI-DSS | If the system handles credit card data |

**Common thread across all of them:** they care about *personal data* being collected, processed, stored, transmitted, and deleted responsibly.

---

## 4. What Counts as "Personal Data"

Treat the following as personal data by default:

- Names, email addresses, phone numbers, postal addresses
- Government IDs (CNIC, SSN, NHS number, passport, etc.)
- IP addresses, device identifiers, cookies tied to a user
- Geolocation
- User credentials (passwords, tokens, recovery answers)
- Payment information (card numbers, bank details)
- Behavioral data linked to an identifiable user (clicks, search history)
- Anything that, alone or combined, could identify a specific person

**When in doubt: assume it's personal data.** Treat it accordingly.

---

## 5. Engineering Practices Required by This Framework

These practices are non-negotiable and enforced via code review + CI where possible.

### 5.1 Secrets Management
- ❌ **Never** commit API keys, tokens, passwords, or credentials to the repo
- ✅ Use environment variables (`.env`) — and `.env` must be in `.gitignore`
- ✅ Provide an `.env.example` with the variable names but no real values
- ✅ Production secrets live in a secret manager (e.g., AWS Secrets Manager, Vault, GitHub Actions secrets) — never in source code
- ✅ If a secret is ever committed by accident: **rotate it immediately**, then remove from history

### 5.2 PII (Personally Identifiable Information) Handling
- ❌ **Never** log PII in plaintext — no full emails, names, or addresses in logs
- ❌ **Never** use real user data in tests, dev databases, or screenshots
- ✅ Use synthetic/fake data in fixtures
- ✅ Log identifiers (user IDs) but not the personal data behind them
- ✅ If PII must appear in logs for debugging, mask it (e.g., `m***@example.com`)

### 5.3 Data Minimization
- Collect only what you need
- Store only what you need
- Keep it only as long as you need
- Document **why** each field is collected — if no one can answer, don't collect it

### 5.4 Encryption
- **In transit:** all traffic over HTTPS / TLS. No plain HTTP for anything user-facing
- **At rest:** sensitive data in the database should be encrypted (database-level or field-level for the most sensitive fields)
- **Passwords:** never stored in plaintext — use a strong hash (bcrypt, argon2) with appropriate cost factor

### 5.5 Access Control
- Principle of least privilege — give the minimum access needed
- Authentication required for any non-public endpoint
- Authorization checks at every endpoint that touches user data
- Admin actions logged with who/what/when

### 5.6 User Rights (Privacy by Design)
Most data laws give users the right to:
- **Access** their data ("show me what you have on me")
- **Correct** their data
- **Delete** their data ("right to be forgotten")
- **Export** their data (data portability)

The data model should be designed from day one so these are technically possible. Don't bury user data in places where it can't be retrieved or deleted.

### 5.7 Audit Trail
- Every change to production code is traceable to a PR
- Every PR is traceable to a reviewer and an author
- Every deployment is traceable to a commit
- Git history is the audit trail — never force-push to `main`

This satisfies the spirit of audit requirements in SOX, ISO 27001, and SOC 2 — even though we don't claim formal certification.

### 5.8 Dependency Security
- CI runs `npm audit` on every PR (see `docs/05-ci-cd-quality-gates.md`)
- **High or Critical vulnerabilities block merges**
- Dependencies are reviewed monthly and updated when patches are available
- New dependencies require review — don't `npm install` random packages

---

## 6. Security Severity — P0-Security

In addition to the standard P0–P3 bug severities (see `docs/06-bug-lifecycle.md`), we have a top-level severity for security issues:

### P0-Security
- **What:** Active or potential breach. Exposed credentials. Authentication bypass. Data leak.
- **Examples:** API key in repo, SQL injection vulnerability discovered, user data exposed in public response, account takeover possible
- **Response:**
  1. **Immediately notify Lead + SQA** (private channel, not public)
  2. Rotate any exposed credentials within 1 hour
  3. Contain (patch, take feature offline, etc.)
  4. Assess scope — what data, how many users
  5. If user data was exposed: legal counsel determines notification requirements per applicable law
- **Disclosure:** never publicly until contained and legally cleared

**Why a separate severity:** security issues require different handling — silent until contained, then transparent. A normal P0 is announced openly to coordinate. A P0-Security is need-to-know until safe.

---

## 7. Cross-Border Data Considerations

If our users are in multiple regions, the strictest law generally applies:

- **EU/UK users → GDPR/UK GDPR applies** even if the server is in Pakistan/US
- **California users → CCPA applies** even if the company is elsewhere
- **Cross-border data transfer** (e.g., EU data to a US server) may require specific legal mechanisms (Standard Contractual Clauses, adequacy decisions, etc.)

**Engineering implication:** know where users are and know where data is stored. If we cannot answer either question, we cannot answer compliance questions.

---

## 8. What This Framework Does NOT Cover

This framework intentionally does not claim or cover:

- Formal GDPR Data Protection Impact Assessments (DPIAs)
- Records of Processing Activities (ROPAs) required under GDPR Article 30
- Data Processing Agreements (DPAs) with vendors
- Privacy Policy / Terms of Service drafting
- Cookie consent banner legal sufficiency
- Breach notification procedures (the *technical* response is here; legal notification is not)
- HIPAA Business Associate Agreements
- PCI-DSS attestation
- SOC 2 readiness audits

**All of the above require legal/compliance expertise the framework does not provide.**

---

## 9. Pre-Production Readiness Checklist (Compliance)

Before the app processes real user data in production, the following should be in place (separately from this framework):

- [ ] A Privacy Policy reviewed by legal counsel
- [ ] Terms of Service reviewed by legal counsel
- [ ] Clear understanding of which jurisdictions users are in
- [ ] Data storage locations documented
- [ ] Vendor list reviewed (any third-party processing user data)
- [ ] Breach notification process defined (who decides, who notifies, on what timeline)
- [ ] User rights endpoints implemented (access, correct, delete, export)
- [ ] Cookie consent mechanism if serving EU/UK users
- [ ] Age-gating if minors might use the service

**This list is for awareness — not engineering scope.**

---

## 10. What Engineers Should Do When in Doubt

1. **Default to less data, more encryption, narrower access.**
2. **If you're not sure something is okay, ask SQA or Lead before merging.**
3. **If you discover a vulnerability or exposure, treat it as P0-Security immediately.**
4. **Document data flows** — if asked "where does email go after a user signs up?" you should be able to answer.
5. **Read the linked external resources** (below) when adding new categories of data handling.

---

## 11. References for Self-Education

- [GDPR official text (eur-lex)](https://eur-lex.europa.eu/eli/reg/2016/679/oj)
- [UK ICO Guide to Data Protection](https://ico.org.uk/for-organisations/uk-gdpr-guidance-and-resources/)
- [OWASP Top 10](https://owasp.org/www-project-top-ten/) — the most common web vulnerabilities
- [OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/)
- [Pakistan PDPB draft text](https://moitt.gov.pk/) — Ministry of IT (status pending at time of writing)
- [California CCPA](https://oag.ca.gov/privacy/ccpa)

---

## 12. Closing Note

This document gives the team a defensible baseline. It is honest about what it covers and what it does not. Adding a "GDPR ✅" badge without doing the legal work would be worse than this framework — it would create liability.

The goal: when someone asks "what does your team do about compliance?", any developer can point to this document and answer accurately.

— Rao Maarij Hassan, SQA

---

*End of document. Continue to [`checklists/`](../checklists/) for operational checklists.*
