# CertainLogic Hallucination Benchmark

**30 real hallucination test cases.** Published April 2026. Run them against any LLM.

[![Tested by CertainLogic](https://img.shields.io/badge/Tested%20by-CertainLogic-blue?style=flat-square)](https://certainlogic.ai)
[![License: BSL 1.1](https://img.shields.io/badge/License-BSL_1.1-blue?style=flat-square)](LICENSE)
[![Cases: 30](https://img.shields.io/badge/Test%20Cases-30-orange?style=flat-square)](cases/)

---

## Study Context

Published April 2026. 30 factual questions across medical, legal, financial, technical, and general knowledge. Each case has a single verifiable correct answer.

These test cases measure **confident incorrect responses** — the failure mode where an LLM sounds sure but is wrong.

> **Regulated Industries Disclaimer:** This benchmark evaluates factual correctness on a limited set of test cases. It is not a clinical, legal, financial, or compliance validation. The test cases and results are provided for research and benchmarking purposes only and do not constitute professional advice. Do not use these results as the sole basis for deployment decisions in regulated environments.

> ⚠️ **Conflict of Interest:** CertainLogic Brain API is developed by the authors of this benchmark. All Brain API results are proprietary and reserved for NDA review. The public results below are limited to independently reproducible bare-LLM runs. See the end of this section for NDA-only context.

## Public Benchmark Results (April 2026 Run)

| System | Medical | Legal | Financial | Technical | General | **Overall** |
|--------|---------|-------|-----------|-----------|---------|-------------|
| GPT-4o (bare) | 60% | 80% | 60% | 80% | 90% | **77%** |
| Claude 3.5 Sonnet (bare) | 80% | 80% | 60% | 80% | 90% | **80%** |
| Llama 3.3 70B (bare) | 60% | 60% | 60% | 80% | 80% | **70%** |
| Claude Opus 4 | ~100% | ~100% | ~100% | ~100% | ~100% | ~100% |

All above results independently reproducible via live API calls with your own keys. Test cases, scoring criteria, and runner code included in this repository.

### Internal / NDA-Only Context

CertainLogic Brain API was evaluated on the same 30 cases during an April 2026 internal run. Results and methodology are proprietary — not independently verifiable without NDA. Contact anton@certainlogic.ai for NDA access.

> Full results with per-case breakdowns: [`results/certainlogic_results.json`](results/certainlogic_results.json)

---

## Why This Matters

LLMs hallucinate on factual questions in ways that are predictable and dangerous. Models that ace reasoning benchmarks will confidently state wrong retirement ages, unsafe drug combinations, or incorrect capital cities. These are systematic failure modes that appear consistently across providers.

## How to Run

```bash
# 1. Install dependencies
pip install -r requirements.txt

# 2. Run against your model
python benchmark.py --provider openai --model gpt-4o --api-key YOUR_KEY

# 3. Add auto-scoring
python benchmark.py --provider anthropic --model claude-3-5-sonnet-20241022 --api-key YOUR_KEY \
  --evaluator-provider openai --evaluator-model gpt-4o --evaluator-key YOUR_OPENAI_KEY
```

**Supported providers:** `openai`, `anthropic`, `openrouter`

Results saved to `results/<provider>_<model>_<timestamp>.json`.

No CertainLogic account required.

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
> *Q: What is the full retirement age for Social Security benefits for someone born in 1960?*
> *Correct: 67. Common hallucination: 65 (outdated figure).*

**Medical — Drug interaction** (`med-002`)
> *Q: Is it safe to take ibuprofen and low-dose aspirin together?*
> *Correct: No — ibuprofen reduces aspirin's cardioprotective effect. Common hallucination: "Yes, both are NSAIDs and safe together."*

## License

Business Source License 1.1 (BSL 1.1) — broad grant. See LICENSE.

---

*CertainLogic Research | Published April 2026*
