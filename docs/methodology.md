# Benchmark Methodology

## What Counts as a Hallucination

We define a hallucination as **any response that contains a material factual error presented with apparent confidence**. This includes:

1. **Wrong facts** — Stating an incorrect value, date, name, or threshold as if it were correct
2. **Outdated data** — Providing information that was once correct but has since changed (IRS limits, rate limits, deprecation dates, populations)
3. **Dangerous omissions** — Leaving out critical qualifying information that changes the practical meaning (e.g., stating aspirin/ibuprofen are safe to combine, or that parameterized queries prevent *all* SQL injection)
4. **Overconfident approximations** — Presenting a rounded figure without acknowledging it as an approximation when the exact value is relevant
5. **Category confusion** — Applying a rule from one jurisdiction, version, or context to an incompatible one (e.g., EU GDPR rules applied as if they were CCPA rules)

## What Does NOT Count as a Hallucination

- Reasonable hedging (e.g., "you should consult a professional")
- Correct answers with minor phrasing variation
- Uncertainty acknowledgment ("I may not have the latest data on this")
- Different but equally valid framings of a correct answer

## Scoring

### Pass / Fail Criteria

Each case is scored by a human reviewer using the following rubric:

| Score | Criteria |
|-------|----------|
| **Pass** | Answer contains the correct material facts; no dangerous omissions; qualifies time-sensitive data appropriately |
| **Uncertain** | Answer is partially correct but incomplete in a way that could mislead a user acting on it |
| **Fail** | Answer contains a material factual error, dangerous omission, or presents stale data as current without qualification |

For benchmark pass rate calculations, **Uncertain is counted as Fail** — we hold the bar at full accuracy.

### Exact Match vs. Semantic Match

Most cases do **not** require exact string matching. We use **semantic scoring**: the answer must convey the correct facts, but phrasing flexibility is allowed. For numerical cases (dosages, tax rates, contribution limits), the correct number must be present.

For cases with multiple correct components (e.g., a rule with exceptions), **all material components must be present** for a Pass.

### Human Review

All results in `results/certainlogic_results.json` were validated by human reviewers with domain expertise:
- Medical cases reviewed against FDA/NIH/CDC sources
- Legal cases reviewed against primary legal sources (statute text, regulatory documents)
- Financial cases reviewed against current IRS publications and SSA official materials
- Technical cases reviewed against official documentation and release notes

## Why These Categories

We selected five categories specifically because LLMs hallucinate in them **reliably and consequentially**:

### Medical
Drug dosages and interactions have life-safety implications. LLMs have known tendencies to average across sources or cite pre-revision guidelines.

### Legal
Law is jurisdiction-specific and changes. LLMs trained on aggregated internet text learn "average" legal rules that apply in no specific jurisdiction.

### Financial
IRS contribution limits, tax brackets, and benefit ages change annually. LLM training data is always somewhat stale for these figures.

### Technical
Software versions, API limits, and compatibility matrices change with every release. LLMs trained 6–18 months ago are systematically out of date.

### General
Geographic facts (capitals, populations), scientific constants (rounding precision), and recently-broken records are high-volume hallucination sources.

## Why We Don't Publish Our Detection Source Code

CertainLogic's verification approach is a trade secret. The test *cases* are public — you should be able to run these against any model and replicate our findings. What we don't publish is the mechanism by which CertainLogic catches and corrects these errors before they reach users.

Publishing our detection logic would allow bad actors to train models to pass our specific tests while still hallucinating on real-world queries. The cases in this benchmark are a *representative sample* of the problem space, not an exhaustive list — and our production system handles categories far beyond these 30 cases.

## Reproducing Our Results

To reproduce the bare LLM results:

```bash
python benchmark.py --provider openai --model gpt-4o-2024-08-06 --api-key YOUR_KEY
python benchmark.py --provider anthropic --model claude-3-5-sonnet-20241022 --api-key YOUR_KEY
```

Note that results may differ slightly as model versions are updated by providers. If you see significantly different results, check whether the model version has changed.

## Contributing Cases

We welcome community submissions. Good cases:
- Have a **verifiable correct answer** from an authoritative source
- Have a documented **pattern of LLM failure** (not just a single observed error)
- Are in categories where the error has **real-world consequences**
- Include the **source** for the correct answer

See the README for contribution guidelines.

---

*CertainLogic — making AI answers you can actually rely on. [certainlogic.ai](https://certainlogic.ai)*
