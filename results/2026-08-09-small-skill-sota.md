# Daily Small-Skill SOTA — 2026-08-09

Status: **AUDIT: PASS**  
Validated findings: **2**  
Model setting shown in the Work chat: **GPT-5.6 Luna, Extra High**  
Research and same-chat audit: [Work chat](https://chatgpt.com/c/6a780254-833c-83e9-a4a7-075f16c01ac2)

The quality gate was used as an internal research-and-repair loop. It did not
block the visible result. Both findings are NEW against the canonical ledger
and independent receipt history.

## 1. SelfCompact with Qwen3-4B

Verdict: **NEW — Evidence grade B**

- Public artifact: `tianjianl/selfcompact@12958172dacb77e71883d189b40638f693cd20de`
- Core implementation commit: `8c1f5e30d146304720126d74a58b0cbcb4c197c7`
- Paper: [SelfCompact](https://arxiv.org/abs/2606.23525), first published 2026-06-22
- Code: [tianjianl/selfcompact](https://github.com/tianjianl/selfcompact)
- Reproduction model pin: `Qwen/Qwen3-4B-Instruct-2507@cdbee75f17c01a7cc42f958dc650907174af0554`
- Exact learned parameters: 4,022,468,096
- License: Apache-2.0 for the pinned Qwen model

### What changed

The same model emits a compaction marker only when it judges that a subproblem
has converged. This avoids compressing unresolved intermediate reasoning while
still allowing much longer trajectories. It requires neither a separate
summarizer nor fine-tuning.

### Controlled result

The paper evaluates IMO-Answerbench, HMMT November 2025, and HMMT February
2026, with 16 samples per problem under the same vLLM and scoring protocol.

| Qwen3-4B condition | Mean score | Average tokens |
|---|---:|---:|
| No compaction | 38.7 | 16k |
| Fixed-interval compaction | 41.5 | 44k |
| SelfCompact | **45.1** | 48k |

The defensible claim is a **+3.6 point capability gain over token-budget-matched
fixed compaction**, not a cost or latency improvement. SelfCompact consumes
roughly three times the tokens of the no-compaction run.

### Reproducible workflow

Run the published implementation through vLLM with temperature 1.0, top-p
0.7, a 16,384-token round cap, a maximum of 12 rounds, a 512-token summary
cap, and the pinned Qwen3-4B revision. Accept the result only if the scored
answers reproduce the improvement over fixed-interval compaction under the
same question set and token policy.

### Limits

- Published 4B experiments used one H200.
- Wall-clock latency was not reported.
- Evidence is concentrated in long-horizon math and search-style reasoning.
- The model revision above is a reproducibility pin; the paper does not prove
  that exact Hugging Face commit was the original experimental weight revision.

## 2. Transformers CLI+Skill clone-context efficiency regression

Verdict: **NEW negative finding — Evidence grade B**

- Study: [Is Transformers Agentic Enough?](https://huggingface.co/blog/is-it-agentic-enough), 2026-06-18
- Harness: [huggingface/is-it-agentic-enough](https://github.com/huggingface/is-it-agentic-enough)
- Immutable report: `transformers-community/is-transformers-agentic@1742c8775b15d9ac9c792ca226e03a84ffb24043`
- Tested model: the same exact 4,022,468,096-parameter Qwen3-4B revision above
- Compared Transformers revisions: v5.10.2 `8e1d47a81d` and CLI+Skill `4d15b215f3`

### Controlled result

For the Qwen3-4B clone/source-context tier:

| Revision | Runs | Scored | Median new tokens | Median latency |
|---|---:|---:|---:|---:|
| Transformers v5.10.2 | 57 | 42 | 2,448 | 3.4 s |
| CLI+Skill revision | 56 | 41 | 23,011.5 | 23.1 s |

That is a verified **9.4x increase in new-token use** and **6.8x increase in
latency**. The same report's narrower skill-only tier was much cheaper: 3,355
median new tokens and 9.1 seconds.

### Actionable workflow conclusion

For atomic tasks on approximately 4B agents, do not inject an entire mutable
repository and broad CLI examples by default. Start with a curated skill or
minimal callable interface, then add source context only when the acceptance
check demonstrates that it is necessary.

### Audit correction and limits

- The audit rejected the draft's “no accuracy gain” wording: the compared
  conditions had different scored denominators, 26/42 versus 33/41.
- The accepted result is the token and latency regression only.
- Fresh agents per task make this a worst-case discovery cost that may amortize
  in persistent sessions.
- The benchmark covers narrow deterministic Transformers tasks.
- The exact worker SKU was not disclosed, so only the controlled relative
  latency comparison is supported.

## Rejected near-misses

- BAAI AREX-Turbo: no matched 4B backbone control; the useful ablation used a
  122B model.
- InternScience Agents-A1-4B: mixed original-report and rerun scores, plus a
  tau2 regression and no latency/SKU disclosure.
- HyperTool/Qwen3-8B: strong controlled result but no public official
  checkpoint/code artifact was found.
- Nexus-TinyFunction-1.2B-v2, Bonsai-8B, and UI-Venus-1.5: outside the 90-day
  novelty window.
- Qwen3.5-9B: 9,653,104,368 learned parameters, above the hard 9B cap.

Delivery status: **ELIGIBLE_FOR_FUTURE_RELAY**
