# Credibility rubric (install mode) — 100 points

| Area | Pts | How to score (cite a URL for each) |
|---|---|---|
| A. Maintainer & provenance | 25 | Large company or foundation official org 25 · well-known individual or researcher with a verifiable track record 18 · small org 10 · anonymous or new account 0–5 |
| B. Activity & adoption | 15 | Commits or releases in the last 90 days, issue response, known adopters. Stars and forks are weighted low because they can be faked |
| C. Security | 25 | Security scan result, OSV vulnerabilities, OpenSSF Scorecard, GitHub advisories, whether requested permissions (network, files, secrets) are justified |
| D. Independent agreement | 20 | Citation check results plus at least 2 independent sources agreeing (papers, talks, official docs, third-party reviews). Deduct for contradictions |
| E. Fit & cost | 15 | License (commercial use, copyleft acceptable?), OS fit, context or token cost, overlap with tools already installed, maintenance burden |

## Hard fails (reject regardless of score)
- The security gate denies it.
- The repo is archived or abandoned.
- It looks like a typosquat or impersonation.
- Its main claims are contradicted by evidence.
- Its license forbids the intended use.

## Verdict
- **≥80** recommend.
- **60–79** conditional: list the concerns and let the human decide.
- **<60** do not recommend.

Report the score as `A x/25 · B x/15 · C x/25 · D x/20 · E x/15 = total`.
