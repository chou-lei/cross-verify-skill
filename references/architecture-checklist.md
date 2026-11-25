# Architecture completeness checklist (architecture mode)

Answer every item, or mark it "deliberately out of scope" with a reason. "Complete" means every item is addressed, not that every feature is added.

1. **Requirements coverage.** Map each requirement to the component that serves it.
2. **Reference architectures.** Compare against at least 2 (papers, official docs, or talks by recognized engineers). List what they have that this design lacks, and why.
3. **Failure modes.** What breaks, how it is detected, how to recover or roll back.
4. **Security & privacy.** Secrets, personal data, least privilege, prompt injection.
5. **Data & state.** Where it lives, backups, consistency.
6. **Verification.** How each component is tested. Done conditions must be machine-checkable.
7. **Observability.** Logs, usage and cost tracking, error reporting.
8. **Cost.** Tokens, money, maintenance time.
9. **Dependencies & licensing.** Each dependency passes `rubric.md`.
10. **Non-goals.** What this design explicitly will not do.

Mark each item ✔ covered / ⚠ partial / ✘ missing / — out of scope, with the evidence URL used.
