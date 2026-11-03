# Fine-Tuning Interview Questions (SFT, LoRA, QLoRA, RLHF, DPO)

> When prompting/RAG isn't enough: SFT, LoRA/PEFT, DPO, distillation — and when NOT to fine-tune.

[◂ Back to the handbook](../README.md)

---

## The adaptation ladder: prompt → RAG → fine-tune
_~6 min_

There's an ordered ladder of ways to make a model do what you want. Climb it **in order** — each rung is more expensive and less flexible than the last.

1. **Prompt engineering** — cheapest, instant to change. Try first.
2. **Few-shot / dynamic examples** — teach by demonstration in-context.
3. **RAG / tools** — add knowledge and abilities at runtime.
4. **Fine-tuning** — change the weights. Last resort, for when the above plateau.

> 🎯 **Fine-tuning does NOT add knowledge (mostly)** — The classic mistake: 'I'll fine-tune to teach the model our docs.' Fine-tuning is for **behavior, format, tone, and narrow skills** — not for injecting a knowledge base (that's RAG). Fine-tuning on facts is expensive, forgetful, and un-citable.

- Fine-tune when: you need a **consistent style/format** prompting won't hold, a **specialized narrow task** done cheaply/fast, lower latency/cost via a smaller specialized model, or to bake in a hard-to-express behavior.
- Don't fine-tune when: the knowledge changes often (RAG), you have few examples, or you haven't exhausted prompting.

**◆ Lock it in**

- Climb the ladder in order: **prompt → few-shot → RAG/tools → fine-tune**.
- Fine-tuning changes **behavior/format/skill**, not knowledge — use RAG for facts.
- Fine-tune only after prompting/RAG plateau, when you have enough quality examples.

> 🧠 **Feynman drill** — A teammate wants to fine-tune to make the bot know this quarter's prices. Talk them out of it.

**Retrieval check**

**Q: Which is the BEST reason to fine-tune (rather than prompt/RAG)?**

<details><summary>Show answer</summary>

**✓ To reliably enforce a specific output style/format or a narrow skill that prompting can't hold consistently.**

Fine-tuning shines for consistent behavior/format/narrow skills. Volatile facts (inventory, prices, daily docs) belong in RAG, not the weights.
</details>

---

## How fine-tuning works: SFT, LoRA/PEFT, DPO & distillation
_~7 min_

- **SFT (supervised fine-tuning)**: train on (prompt → ideal completion) pairs. The workhorse — you need a curated, high-quality dataset (hundreds to thousands of examples).
- **LoRA / PEFT (parameter-efficient fine-tuning)**: instead of updating all weights, train small **adapter** matrices. Far cheaper, faster, smaller to store, and you can swap adapters per task. The default modern approach.
- **Preference tuning (DPO / RLHF)**: train on **preferred vs rejected** response pairs to shape nuanced behavior/tone. DPO is a simpler, popular alternative to full RLHF.
- **Distillation**: use a big 'teacher' model to generate training data (or targets) for a small 'student' model — get near-big-model quality on a task at small-model cost/latency.
- **RL fine-tuning (RFT / RLVR, e.g. GRPO)**: optimize against a **grader** (tests pass, rubric score, task completed) instead of fixed target outputs. The 2026 frontier for applied teams — e.g. Harvey reported post-training an open-weight model on ~1,750 simulated legal tasks graded against attorney rubrics.

> 💡 **Data quality > data quantity** — Fine-tuning success is dominated by **dataset quality**: clean, consistent, representative examples that demonstrate exactly the behavior you want. A few hundred great examples beat thousands of noisy ones. Garbage in, worse-and-confidently-wrong out.

> ⚠️ **Costs & catastrophic forgetting** — Fine-tuning has real costs: data curation, training, hosting the custom model, re-tuning when the base updates, and **catastrophic forgetting** (the model gets worse at general tasks). Weigh this against staying on a prompted frontier model.

> 💡 **Fine-tuning in late 2026 — the reality check** — Hosted fine-tuning of proprietary models split in 2026: **OpenAI is closing self-serve fine-tuning** (no new jobs after Jan 6, 2027 — its stated reason: new models follow instructions and formats well enough), Anthropic doesn't offer it for current models, while **Google moved the other way** (Gemini supervised fine-tuning GA, RL fine-tuning in preview). Most applied fine-tuning now means **open-weight models** — LoRA SFT and increasingly **RL against graders** on managed platforms (e.g. Thinking Machines' Tinker, Fireworks, Bedrock). Distilling from closed models got harder (hidden reasoning, logprobs removed), so teams distill from **open teachers**. Rung zero still holds: **context-engineer first**. Fine-tune for **form, latency, unit cost and narrow agentic skill**; never for facts.

**◆ Lock it in**

- **SFT** = train on ideal completions; **LoRA/PEFT** = cheap adapters (modern default); **DPO/RLHF** = preference tuning; **RFT/RLVR** = optimize against a grader.
- **Distillation** = teacher model trains a cheaper student for prod cost/latency.
- Success = **data quality**; beware hosting costs, re-tuning, and catastrophic forgetting.
- 2026: hosted FT is **vendor-dependent** (OpenAI closing, Google expanding); most FT = **open weights** — LoRA SFT + RL against graders.

> 🧠 **Feynman drill** — Explain LoRA in one sentence and why it's cheaper than full fine-tuning.

**Retrieval check**

**Q: You want cheap, fast task-specific fine-tuning without paying to update all weights. Use:**

<details><summary>Show answer</summary>

**✓ LoRA / PEFT — train small adapter matrices instead of all weights.**

LoRA/PEFT trains small adapters rather than the full model — cheaper, faster, swappable per task. That's the standard efficient fine-tuning route.
</details>

---

[◂ Back to the handbook](../README.md) · [▸ Practice this interactively](https://deployprep.vercel.app/track/fde)
