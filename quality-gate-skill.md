# Daily Small-Skill External Quality Gate

This file is a read-only external quality-control layer. It must never be merged into, rewritten into, or used to edit an existing Claude-derived research prompt.

## Contract

- The production researcher reports zero to five important new small-skill agents. The count is evidence-driven, never a quota.
- A report is a draft until an independent same-chat auditor returns `AUDIT: PASS`.
- `QUIET` is valid only when invocation, coverage, benchmark, novelty, and both deduplication histories are proven. Missing evidence is `INCOMPLETE`, never quiet.
- The auditor is read-only. It does not edit prompts, schedules, ledgers, or research reports and does not silently repair them.

## Required gates

1. Invocation: distinguish a genuine scheduled run from a manual message.
2. Coverage: attempt all mandatory source classes and record each URL, status, timestamp, candidate count, and error.
3. Artifact: verify the public artifact, exact immutable revision, and downloadable weights or explicit access terms.
4. Size: when a model is claimed, verify total learned parameters are <=9,000,000,000; marketed names and quantized storage are not proof. For software/harness claims, record this gate as N/A with evidence.
5. Benchmark: use a current, task-specific, same-protocol benchmark and comparator. Grade A for independent current evidence and B for official comparable evidence; C is provisional and cannot be reported.
6. Novelty: establish a release, material same-protocol gain, >=2x measured efficiency gain, or a verified transition from cloud/slow/lab-only to public local or real-time capability since the 90-day baseline.
7. Atomic usefulness: state one callable input, one verifiable output, the worker, workflow, and acceptance check.
8. Reproducibility: record revision, license/access, hardware, precision, context/resolution, command/demo, and runtime when making local or real-time claims.
9. Deduplication: compare the union of the canonical ledger and independent audit receipts by canonical URL, artifact/revision, atomic function, benchmark protocol, and normalized semantic claim. A renamed article, mirrored checkpoint, quantization, or unchanged claim is a duplicate. A material verified delta is an UPDATE linked to its predecessor.

## Durable state

The task chat is the persistent cloud record. On every PASS, emit both records in the same response, in this order:

```text
CANONICAL_LEDGER_RECORD {"record_type":"reported_item","schema_version":1,"report_id":"<uuid>","report_date_bogota":"<YYYY-MM-DD>","report_locator":"<task-chat message>","atomic_function":"<input -> output>","artifact_key":"<org/repo@revision>","canonical_urls":["<primary URL>"],"benchmark_claim_key":"<benchmark|version|split|protocol|metric|score|comparator>","semantic_claim":"<normalized capability delta>","evidence_grade":"A|B","update_of":null,"previous_receipt_hash":null,"receipt_hash":"<sha256 of canonicalized record without receipt_hash>"}
DEDUP_RECEIPT {"report_id":"<uuid>","artifact_key":"<org/repo@revision>","benchmark_claim_key":"<claim key>","report_locator":"<task-chat message>","previous_receipt_hash":null,"receipt_hash":"<same hash>"}
```

Never emit either record for a draft, near-miss, failed run, incomplete run, or unverified item. Before PASS, compare all prior `CANONICAL_LEDGER_RECORD` and `DEDUP_RECEIPT` messages. If one history is missing or they diverge, return `INCOMPLETE` and `HOLD`.

## Required result

Every researcher run must visibly include:

```text
RUN: <run-id> | <scheduled|manual-test> | <ISO timestamp> | America/Bogota
MODEL: <model> | EFFORT: xhigh
SOURCES: attempted=<n> succeeded=<n> failed=<n> coverage=<percent>
STAGES: discovered=<n> deduped=<n> artifact/release=<n> measured=<n> novel90d=<n> useful=<n> finalists=<n> reported=<n>
BUDGET: searches=<n> fetches=<n> ceiling=<n>
STATE: canonical-ledger=<available|missing|unreadable> backup-receipts=<available|missing|unreadable> reconciled=<yes|no|unknown>
```

The auditor must output exactly one of `AUDIT: PASS`, `AUDIT: FAIL`, or `AUDIT: INCOMPLETE`, followed by evidence, gate failures, source failures, dedup status, and `Delivery status: ELIGIBLE_FOR_FUTURE_RELAY` only for PASS; otherwise `Delivery status: HOLD`.
