# Cloud research state and results

This repository contains the external quality gate, the canonical finding
ledger, the independent backup receipt history, and durable reports from the
Work cloud research tasks.

The gate is an internal verification loop. It does not suppress the final
research output. Researchers produce evidence-driven zero-to-five-item drafts;
same-chat auditors keep verifying or remove weak candidates until they can
return `AUDIT: PASS` over the validated subset.

The canonical ledger and backup receipts are intentionally independent, not
byte-identical. They are reconciled by `report_id`, `artifact_key`,
`benchmark_claim_key`, `previous_receipt_hash`, and `receipt_hash`.

## Published reports

- [2026-08-09 Small-Skill SOTA](results/2026-08-09-small-skill-sota.md)
- [2026-08-09 Harness and Plugins SOTA](results/2026-08-09-harness-plugins-sota.md)
