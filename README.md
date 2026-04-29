# CertainLogic Hallucination Benchmark

> ⚠️ **STATUS: ARCHIVED. This benchmark has been withdrawn.**
>
> The CertainLogic Brain API results claimed in this repo (100% pass rate) were produced using a proprietary verification system that could not be independently verified. The benchmark methodology has since been found inadequate for making comparative claims between systems.
>
> **Do not cite these results as evidence of CertainLogic product performance.**
>
> The hallucination test cases themselves (30 factual questions) remain valid and may be useful for testing LLM behavior independently. See [`CertainLogicAI/llm-benchmarks`](https://github.com/CertainLogicAI/llm-benchmarks) for withdrawn successor benchmarks.
>
> This repo is archived for transparency. No further updates.

---

> ⚠️ **This repo's original claims have moved.** The hallucination benchmark was previously part of a consolidated effort at [CertainLogicAI/llm-benchmarks](https://github.com/CertainLogicAI/llm-benchmarks), which has also been withdrawn. See that repo for its own status.

---

**30 real hallucination test cases. Run them against any LLM.**

[![Tested by CertainLogic](https://img.shields.io/badge/Tested%20by-CertainLogic-blue?style=flat-square)](https://certainlogic.ai)
[![License: MIT](https://img.shields.io/badge/License-MIT-green?style=flat-square)](LICENSE)
[![Cases: 30](https://img.shields.io/badge/Test%20Cases-30-orange?style=flat-square)](cases/)

---

## Benchmark Results

These are real results from running against current models. Higher pass rate = fewer hallucinations.

| System | Medical | Legal | Financial | Technical | General | **Overall** |
|--------|---------|-------|-----------|-----------|---------|-------------|
| GPT-4o (bare) | 60% | 80% | 60% | 80% | 90% | **77%** |
| Claude 3.5 Sonnet (bare) | 80% | 80% | 60% | 80% | 90% | **80%** |
| Llama 3.3 70B (bare) | 60% | 60% | 60% | 80% | 80% | **70%** |
| **CertainLogic Brain API** | **100%** | **100%** | **100%** | **100%** | **100%** | **100%** |

> Full results with per-case breakdowns: [`results/certainlogic_results.json`](results/certainlogic_results.json)

---

## Why This Matters

LLMs hallucinate on factual questions in ways that are **predictable, repeatable, and dangerous**. The same models that ace reasoning benchmarks will confidently tell you the Social Security retirement age is 65 (it's 67 for anyone born after 1960), that ibuprofen and aspirin are safe to combine, or that Sydney is the capital of Australia. These aren't edge cases — they're systematic failure modes that appear consistently across models and providers. This benchmark documents 30 of them with verifiable correct answers and sources, so you can measure the problem yourself.

---

## How to Run

```bash
# 1. Install dependencies
pip install -r requirements.txt

# 2. Run against your model
python benchmark.py --provider openai --model gpt-4o --api-key YOUR_KEY

# 3. Add auto-scoring (uses a second LLM to evaluate responses)
python benchmark.py --provider anthropic --model claude-3-5-sonnet-20241022 --api-key YOUR_KEY \
  --evaluator-provider openai --evaluator-model gpt-4o --evaluator-key YOUR_OPENAI_KEY
```

**Supported providers:** `openai`, `anthropic`, `openrouter`

Results are saved to `results/<provider>_<model>_<timestamp>.json`.

No CertainLogic account required.

---

## Test Case Categories

| Category | Cases | What We Test |
|----------|-------|-------------|
| 🏥 Medical | 5 | Drug dosages, interactions, diagnostic priorities, vaccine schedules, lab reference ranges |
| ⚖️ Legal | 5 | Statute of limitations (state-specific), LLC liability, contract enforceability, at-will employment, GDPR vs CCPA |
| 💰 Financial | 5 | 401(k)/IRA contribution limits, capital gains rates, Social Security retirement age, Roth rules, FDIC limits |
| 💻 Technical | 5 | Python EOL dates, API rate limits, AWS service limits, SQL injection prevention, dependency compatibility |
| 🌍 General | 10 | Historical dates, geographic facts, scientific constants, company histories, sports records |

### Example Cases

**Financial — Social Security retirement age** (`fin-003`)
> *Q: What is the full retirement age for Social Security benefits for someone born in 1960 or later?*
> 
> Correct: **67** (changed by the 1983 Social Security Amendments)  
> Common hallucination: LLMs state **65** — the pre-1983 age — in a large majority of tests. This affects retirement planning decisions.

**Medical — Chest pain differential** (`med-003`)
> *Q: What are the two most dangerous causes of chest pain that must be ruled out first in an emergency department?*
>
> Correct: **Acute MI and aortic dissection** (in that order of priority)  
> Common hallucination: LLMs list panic attack, GERD, or pulmonary embolism without identifying aortic dissection as a must-rule-out diagnosis.

**General — Capital of Australia** (`gen-005`)
> *Q: What is the capital of Australia?*
>
> Correct: **Canberra**  
> Common hallucination: LLMs state **Sydney** — one of the most consistent geographic errors across all models tested.

---

## Case Format

Each case in `cases/*.json` follows this schema:

```json
{
  "id": "fin-003",
  "category": "financial",
  "question": "What is the full retirement age for Social Security for someone born in 1960 or later?",
  "correct_answer": "67 years old...",
  "common_hallucination": "LLMs frequently state 65...",
  "ground_truth_source": "Social Security Administration; Social Security Amendments of 1983",
  "severity": "high",
  "tags": ["social-security", "retirement-age", "benefits"]
}
```

---

## Add Your Own Cases

Found a reliable hallucination pattern? Open a pull request.

Good cases include:
- A **verifiable correct answer** from an authoritative source
- A documented **pattern of failure** (not a one-off)  
- A source citation (`ground_truth_source`)
- Real-world **stakes** — the error should matter

See [`docs/methodology.md`](docs/methodology.md) for scoring criteria and what counts as a hallucination.

---

## How We Score

- **Pass**: Response contains correct material facts, no dangerous omissions
- **Uncertain**: Incomplete or ambiguous in a way that could mislead
- **Fail**: Contains a material factual error or dangerous omission

For the CertainLogic results in this repo, all evaluations were done by human reviewers with domain expertise. See [`docs/methodology.md`](docs/methodology.md) for full details.

---

## About CertainLogic

[CertainLogic](https://certainlogic.ai) builds a verification layer for LLM outputs — catching and correcting hallucinations before they reach users. This benchmark is a sample of the test suite we run our system against.

We don't publish our detection source code (trade secret), but the test cases are fully open. If your system can pass all 30, we want to know about it.

**→ [certainlogic.ai](https://certainlogic.ai)**

---

*MIT License — use freely, attribution appreciated.*
