# LLM Evaluation (Evals) Interview Questions & Answers

> The skill that separates hobbyists from engineers: measuring LLM quality rigorously.

[◂ Back to the handbook](../README.md)

---

## Why evals are the #1 applied-AI skill
_~6 min_

LLM outputs are non-deterministic and open-ended, so you **cannot** tell if a change helped by eyeballing a few examples. **Evaluations (evals)** are systematic tests that score model output against expectations. Teams that win at applied AI are the ones that **measure**.

- Without evals you're 'prompt-and-pray' — every change is a guess and regressions are invisible.
- Evals turn 'it feels better' into 'accuracy went 82% → 89% on 200 held-out cases.'
- They enable **iteration speed**: change prompt/model/retrieval, re-run the eval, keep what wins.

> 🎯 **This is THE differentiator** — When an interviewer asks 'how do you know your system is good?' the wrong answer is 'we tested it manually.' The right answer starts with 'I build an eval set from real examples and measure against it on every change.' Lead with evals.

**◆ Lock it in**

- You can't eyeball non-deterministic output — **evals make quality measurable**.
- Evals catch **regressions** and enable fast, safe iteration.
- 'How do you know it's good?' → **lead with your eval strategy**.

> 🧠 **Feynman drill** — Explain to a PM why 'it looked good in my three tests' is not evidence the AI feature works.

**Retrieval check**

**Q: Why are evals especially critical for LLM systems (vs traditional deterministic software)?**

<details><summary>Show answer</summary>

**✓ Output is non-deterministic and open-ended, so systematic measurement is the only reliable way to detect quality changes and regressions.**

Non-determinism + open-ended outputs mean manual spot-checks miss regressions. Evals give you a reliable, repeatable quality signal to iterate against.
</details>

---

## Error analysis: where evals come from
_~7 min_

The 2026 eval bar moved past 'have a golden set.' The method interviewers now expect (codified by Hamel Husain & Shreya Shankar, and echoed in frontier-lab guidance on agent evals) starts with **error analysis**: you **discover** what to measure by reading real failures — you don't invent metrics in a vacuum.

1. **Collect real traces** (production logs, or realistic pilot traffic). ~100 is a **floor, not a target**: keep reading until new traces stop adding failure categories (**saturation**).
2. **Open coding**: read each trace and write a free-form note on the **first upstream failure** only (don't log downstream symptoms of the same root cause).
3. **Axial coding**: cluster the notes into a small **failure taxonomy** ('retrieved wrong policy,' 'ignored refund limit,' 'wrong tone') and **count** them — now you know what actually breaks, and how often.
4. **Build one binary pass/fail judge per failure mode**, validated against human labels. Fix the biggest bucket, re-run, repeat.

- **Hand-code the first ~30 traces yourself** before letting an LLM help cluster the rest — otherwise you inherit its blind spots.
- **Trace-to-dataset**: every production failure becomes a permanent regression test. A great eval suite is **harvested from traffic, not authored up front**.
- **Guardrails ≠ evals**: guardrails are synchronous, deterministic, inline blocking (PII filters, schema checks); evals are async quality **measurement** feeding the improvement loop. Naming this distinction reads senior.
- As capabilities stabilize, migrate saturated **capability evals** into the always-run **regression suite** that gates deploys.

> 🎯 **The eval-harness design round** — 2026 design rounds increasingly make the **measurement system** the deliverable — 'how would you evaluate this agent?' can be the entire hour. The winning shape, said as a loop: **error analysis → failure taxonomy → binary judges validated against human labels → CI regression gate → online monitoring → production traces feed the dataset**.

> ⚠️ **Don't write evals for imagined failures** — Pre-authoring dozens of evaluators before you've read traces is a named anti-pattern — you'll measure what you imagined instead of what breaks. Start with a small smoke set, then grow the real suite from observed failures. Teams that ship well spend **the majority of their applied-AI time** in this loop.

**◆ Lock it in**

- Evals are **discovered, not invented**: real traces → open coding (first upstream failure) → counted failure taxonomy → binary judges.
- Validate each judge with **TPR/TNR on held-out human labels**; prefer **binary** checks per failure mode.
- Wire the loop: **trace → dataset → CI gate → online monitoring**. Guardrails block inline; evals measure async.

> 🧠 **Feynman drill** — Walk through running error analysis on a support bot's last 100 conversations — what do you write down per trace, and what artifact do you end up with?

**Retrieval check**

**Q: Your team wants to 'add evals' to a new LLM feature. Per 2026 best practice, what's the first step?**

<details><summary>Show answer</summary>

**✓ Read ~100 real traces, note the first upstream failure in each, and cluster them into a counted failure taxonomy.**

Error analysis comes first: real traces → open coding → taxonomy. Judges are then built **per observed failure mode** and validated against human labels. Imagined-failure evaluators and self-ratings measure the wrong things; public benchmarks don't reflect your task.
</details>

---

## Building an eval set & choosing metrics
_~7 min_

#### Build the dataset

- Collect **real** inputs (from logs, the customer, or realistic synthetic ones), covering common cases and **edge cases**.
- For each, define the **expected output** or a rubric for what 'good' means.
- Start small — even **20–50 well-chosen cases** beats none — and grow it as you find failures (every bug becomes a new test).

#### Pick the right metric for the task

- **Deterministic checks**: exact match, JSON-valid, regex, does-it-contain, code-runs/tests-pass. Cheap and reliable — use whenever the task allows.
- **Classification metrics**: accuracy, precision/recall/F1 for labeling tasks.
- **Reference-based**: similarity to a gold answer (embedding similarity; older: BLEU/ROUGE).
- **Human evaluation**: the gold standard for subjective quality — but slow and costly; sample it.
- **RAG-specific**: **context relevance** (did we retrieve the right chunks?), **faithfulness/groundedness** (is the answer supported by context?), **answer relevance**.

> 💡 **Decompose the metric** — For RAG, evaluate **retrieval and generation separately**. If answers are wrong, you need to know whether retrieval failed or the model ignored good context. One aggregate score hides the cause.

> 💡 **Leaderboards don't transfer** — Public benchmarks churn and saturate — SWE-bench Verified was retired as a headline coding metric in 2026 in favor of harder suites, and the same model's score can swing by tens of points with the harness around it. Benchmarks pick the **shortlist**; **your** eval set, on **your** data, picks the winner — and makes every model migration a measured change.

**◆ Lock it in**

- Build an eval set from **real + edge-case** inputs with expected outputs/rubrics; grow it from every bug.
- Prefer **deterministic checks**; use classification metrics, reference similarity, or human eval as fit.
- For RAG, measure **retrieval and generation separately** (relevance + faithfulness).

> 🧠 **Feynman drill** — You improved a support bot. Describe the eval set and metrics you'd use to prove it got better.

**Retrieval check**

**Q: For a JSON-extraction task, the most reliable primary eval metric is:**

<details><summary>Show answer</summary>

**✓ Deterministic checks: JSON validity plus field-level exact/normalized match against expected values.**

Structured extraction has a ground truth — use deterministic, field-level checks (valid JSON + exact/normalized match). They're cheap, reliable, and unambiguous.
</details>

---

## LLM-as-judge, offline vs online & guardrails against overfitting
_~6 min_

For subjective or open-ended output (summaries, chat helpfulness), use **LLM-as-judge**: a strong model scores outputs against a rubric. Scalable and surprisingly good — with caveats.

- Give the judge a **clear rubric** and ask for a score + justification; consider pairwise comparison (A vs B) over absolute scores.
- Prefer **binary pass/fail checks per failure mode** over 1–10 scores — humans and models both judge binaries far more consistently; decompose 'quality' into independent checks.
- Beware judge **biases**: position bias (favoring first/second), length bias (favoring longer), self-preference. Randomize order; validate the judge against human labels on a sample.
- **Validate the judge like a classifier**: label **100–200** examples per judge, split them into train (few-shot examples) / dev / test, and report **TPR and TNR** on the test split (>90% target) — raw '% agreement' hides failure on the rare class. Then use TPR/TNR to **correct** the failure rates the judge reports in production.
- In CI, prefer **deterministic assertions** wherever code can check the property; reserve LLM judges for what code can't.

#### Offline vs online evaluation

- **Offline**: run your eval set in CI before shipping — catches regressions pre-deploy.
- **Online**: measure in production — user feedback (👍/👎), A/B tests, task-success signals, human review of samples.
- **Guardrail metrics**: track cost, latency, refusal rate, and safety alongside quality — a 'better' answer that's 3× slower/costlier may not ship.
- **Agents**: measure end-to-end task success first (on the final environment state), then step-level diagnostics — where did the first failure happen? Report **pass^k** (all k runs succeed) for the reliability a customer feels, not just pass@k.

> ⚠️ **Don't overfit the eval** — If you tune endlessly against the same 50 cases, you overfit to them. Keep a **held-out set**, refresh cases from real traffic, and watch online metrics — the eval set is a proxy, not the goal.

**◆ Lock it in**

- **LLM-as-judge** scales subjective eval — use rubrics, pairwise comparisons, and watch position/length bias.
- Run **offline** evals in CI and **online** metrics (feedback, A/B, success) in prod; track cost/latency/safety guardrails.
- Keep a **held-out set** and refresh from real traffic to avoid overfitting the eval.

> 🧠 **Feynman drill** — What biases might an LLM judge have, and how would you defend against them?

**Retrieval check**

**Q: You use an LLM to judge which of two summaries is better. A key bias to control for is:**

<details><summary>Show answer</summary>

**✓ Position/length bias — judges often favor the first-shown or the longer answer; randomize order and control length.**

LLM judges exhibit position and length biases. Randomizing presentation order, controlling for length, and validating against human labels keep the judge honest.
</details>

---

[◂ Back to the handbook](../README.md) · [▸ Practice this interactively](https://deployprep.vercel.app/track/fde)
