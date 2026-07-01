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
- Beware judge **biases**: position bias (favoring first/second), length bias (favoring longer), self-preference. Randomize order; validate the judge against human labels on a sample.

#### Offline vs online evaluation

- **Offline**: run your eval set in CI before shipping — catches regressions pre-deploy.
- **Online**: measure in production — user feedback (👍/👎), A/B tests, task-success signals, human review of samples.
- **Guardrail metrics**: track cost, latency, refusal rate, and safety alongside quality — a 'better' answer that's 3× slower/costlier may not ship.

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

[◂ Back to the handbook](../README.md) · [▸ Practice this interactively](https://YOUR-DOMAIN/track/fde)
