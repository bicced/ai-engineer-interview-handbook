# AI Engineer & Forward Deployed Engineer Interview Handbook (2026)

**Last updated: November 3, 2026** · [What's new](#whats-new-november-2026) · [Changelog](./CHANGELOG.md) · [Sources](./SOURCES.md)

A free, open-source guide to **Applied AI Engineer** and **Forward Deployed Engineer (FDE)** interviews. It covers the LLM, RAG, agent, eval and system-design questions you'll be asked, and how each company's loop actually runs.

<p>
<a href="https://github.com/bicced/ai-engineer-interview-handbook/stargazers"><img alt="Stars" src="https://img.shields.io/github/stars/bicced/ai-engineer-interview-handbook?style=social"></a>
<a href="./LICENSE"><img alt="License: CC BY 4.0" src="https://img.shields.io/badge/License-CC%20BY%204.0-blue.svg"></a>
<a href="./CONTRIBUTING.md"><img alt="PRs welcome" src="https://img.shields.io/badge/PRs-welcome-brightgreen.svg"></a>
<img alt="Last updated" src="https://img.shields.io/badge/updated-Nov%202026-orange.svg">
</p>

**[Practice it interactively, free →](https://deployprep.vercel.app)** · **[Add a question you were actually asked →](./CONTRIBUTING.md)**

---

## What is a Forward Deployed Engineer?

A Forward Deployed Engineer embeds with customers to build and ship production AI systems.

Palantir created the role. In 2026 it spread to AI labs, cloud providers and consultancies. OpenAI's version of the title is **AI Deployment Engineer**. An **Applied AI Engineer** does similar work with less time on-site. Both loops test the same things: applied-LLM depth, system design, coding with AI tools, and judgment in front of customers.

## The FDE market in numbers (late 2026)

| Metric | Value | Source |
|---|---|---|
| US FDE job postings, Jan–Sep 2026 | **7,300+** (vs ~1,200 in all of 2025; under 200 in 2023) | Lightcast, via CNBC (Oct 15, 2026) |
| FDE roles open at one time | ~1,260 across ~555 companies | Plank (Oct 18, 2026) |
| Largest posters | Deloitte 872 · Accenture 688 · Google 499 · KPMG 447 | Lightcast, via CNBC (Oct 15, 2026) |
| Median advertised FDE salary | **~$195K**, up ~40% vs 2023 | Lightcast (Oct 2026) |
| Anthropic US FDE posted base salary | $280K–$320K | Anthropic job postings (2026) |
| Employers allowing AI in technical interviews | 46% allow · 20% case by case · 34% ban | CoderPad survey (2026) |

> "AI fluency is a requirement for all SWE jobs." — Google spokesperson, quoted by Business Insider (Oct 20, 2026)

Full citations are in [SOURCES.md](./SOURCES.md). Claims based on candidate reports are labeled **reported** throughout.

## 📚 Topics

| # | Topic | Covers |
|---|---|---|
| 01 | [**The role & the interview loop**](./topics/01-role.md) | what an AAE/FDE does, the 2026 market, titles, salary & offers, how loops are structured |
| 02 | [**LLM interview questions**](./topics/02-llm.md) | tokens, embeddings, context windows, sampling, reasoning models & effort, training incl. RLVR |
| 03 | [**Prompt engineering interview questions**](./topics/03-prompt.md) | few-shot, chain-of-thought, structured outputs and their limits, robust prompting |
| 04 | [**RAG interview questions**](./topics/04-rag.md) | chunking, embeddings, hybrid search (incl. grep + semantic), reranking, RAG evaluation |
| 05 | [**AI agents & MCP interview questions**](./topics/05-agents.md) | function calling, ReAct, building MCP servers & skills, harnesses, build vs rent, multi-agent |
| 06 | [**LLM evaluation (evals) interview questions**](./topics/06-evals.md) | error analysis to saturation, LLM-as-judge validation, agent evals (pass^k) |
| 07 | [**LLM production interview questions**](./topics/07-prod.md) | cost & caching math, inference economics, reliability, model churn, agent security |
| 08 | [**Fine-tuning interview questions**](./topics/08-finetune.md) | SFT, LoRA/QLoRA, RLHF vs DPO, RL fine-tuning, when to fine-tune in late 2026 |
| 09 | [**AI system design interview questions**](./topics/09-sysdesign.md) | a framework plus worked designs, including an insurance-claims agent |
| 10 | [**FDE behavioral & customer-facing**](./topics/10-fde.md) | ambiguity, STAR, customer scenarios, capability transfer |
| 11 | [**Round-by-round playbook**](./topics/11-rounds.md) | decomposition, take-homes, AI-assisted rounds, coding-agent fluency, the discovery call |
| 12 | [**FDE interview process by company**](./topics/12-companies.md) | Anthropic, OpenAI, Palantir, Google/Microsoft/AWS and agent startups, split into official vs reported |

**Plus:** 📋 [**Interview question bank**](./interview-questions.md) (35 questions with strong answers) · ⚡ [**Flashcards**](./flashcards.md) (91 cards).

## What's new (November 2026)

- **New topic: [company loop guides](./topics/12-companies.md).** Anthropic, OpenAI, Palantir, big tech and startups. Each guide separates official policy from candidate reports and dates every source.
- **12 new lessons:**
  - the 2026 FDE market and salary
  - reasoning models and effort
  - building MCP servers and Agent Skills
  - agent harnesses (build vs rent)
  - inference economics
  - a worked agentic system design
  - coding-agent fluency
  - the five company guides
- **Rewritten for late 2026:**
  - the stateless MCP spec (2026-07-28)
  - sampling parameters disappearing on frontier models
  - structured-output caveats
  - prompt-cache write costs and service tiers
  - OpenAI's retired Assistants API, Agent Builder and Evals
  - OpenAI closing self-serve fine-tuning
  - current eval methodology
  - OWASP updates and attacks on the agent harness
- **Corrected:**
  - Sierra's loop (no AI-free round; see [Sierra's own post](./SOURCES.md#company-loops))
  - OpenAI's FDE loop, which varies by team
  - "temperature 0 = deterministic"
  - "LLM APIs are stateless"

Full list: [CHANGELOG.md](./CHANGELOG.md).

## How to use it

1. **Read a topic**, then answer its **retrieval checks** before revealing the answer. Producing the answer is what makes it stick.
2. **Mix flashcards** from different topics. Interleaving beats cramming one topic.
3. **Answer the question bank out loud**, on a timer, then grade yourself.
4. **Practice interactively** in the free trainer: spaced repetition and timed mock interviews at **[deployprep.vercel.app](https://deployprep.vercel.app)**.

## What interviewers test in 2026

1. **Evals first.** "How do you know it works?" is still the question that separates candidates.
2. **The customer before the tech.** Clarify the problem before you draw any architecture.
3. **Driving AI, and checking it.** AI-assisted rounds grade how you verify AI output. Meta marks candidates down for using the assistant as a crutch.

## Contribute a real interview question

The most valuable addition is **a question you were actually asked**. Include the role, the company, the month and year, and the question itself. Loops change every quarter, and dated reports keep this guide honest. See [CONTRIBUTING.md](./CONTRIBUTING.md).

## Cite or reuse

Content is licensed **[CC BY 4.0](./LICENSE)**: use it, share it and adapt it, with credit to this repo. GitHub's **"Cite this repository"** button uses [CITATION.cff](./CITATION.cff).
