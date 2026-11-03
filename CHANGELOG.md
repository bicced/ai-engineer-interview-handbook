# Changelog

These interview loops change every quarter. Each entry lists what changed and why. Sources are in [SOURCES.md](./SOURCES.md).

## 2026-11-03 — November 2026 refresh

### Added
- **Topic 12: [FDE interview process by company](./topics/12-companies.md).** It has guides for Anthropic, OpenAI, Palantir, big tech (Google, Microsoft, AWS) and agent startups. Each guide separates **official** policy from **reported** candidate experience, and ends with a "Sources & confidence" note.
- **New lessons:**
  - [The 2026 FDE market: titles, employers, salary & offers](./topics/01-role.md)
  - [Reasoning models, effort & test-time compute](./topics/02-llm.md)
  - [Building MCP servers & skills (hands-on)](./topics/05-agents.md)
  - [Harnesses & long-running agents: build vs rent](./topics/05-agents.md)
  - [Inference economics: tokens/sec, TTFT & self-host vs API](./topics/07-prod.md)
  - [Worked example: an agent that triages insurance claims](./topics/09-sysdesign.md)
  - [Coding-agent fluency: the new minimum qualification](./topics/11-rounds.md)
- **21 flashcards** (now 91) and **6 bank questions** (now 35). The new ones cover:
  - managed agent platforms
  - model-retirement migrations
  - reasoning effort
  - securing a coding agent in CI
  - Agent Skills
  - fine-tuning in late 2026
- **Files:** [SOURCES.md](./SOURCES.md) (dated sources), [CITATION.cff](./CITATION.cff), and the full CC BY 4.0 legal text in [LICENSE](./LICENSE).

### Changed
- **LLM foundations**
  - Context windows: 1M tokens is now the default. Server-side conversation state is storage, not model memory.
  - Sampling was rewritten. Many frontier reasoning models reject or ignore temperature and top-p. Variance is now controlled with structured outputs, effort settings and validation.
  - Training now covers RLVR.
- **Prompting**
  - Structured outputs are guaranteed by the schema, but they don't enforce numeric bounds.
  - Prefill and forced tool choice are rejected on several 2026 models.
  - Added OpenAI's Decisions API for classification.
- **RAG**
  - Hybrid grep + semantic retrieval for agents.
  - Rerankers now follow instructions and return calibrated scores.
  - Contextual chunk enrichment.
- **Agents**
  - MCP's 2026-07-28 spec: stateless, `input_required` round-trips, deprecations, Client ID Metadata Documents, and Tasks and MCP Apps as extensions.
  - A2A and Agent Skills now sit under the Agentic AI Foundation.
  - Multi-agent token costs: ~15× a chat, about 4× a single agent.
- **Evals**
  - Error analysis now runs until saturation (~100 traces is the minimum).
  - Hand-code ~30 traces before clustering with an LLM.
  - Validate judges on 100–200 labels split into train/dev/test, and correct measured failure rates with TPR/TNR.
  - Prefer deterministic checks in CI.
  - New callout: leaderboards don't transfer to your use case.
- **Production**
  - Cache writes now cost money at some providers; service tiers.
  - Branch on the error type: a 429 slow-down, a 503 overload and a spend cap each need different handling.
  - Model churn.
  - New section: the agent harness as an attack surface (repo files, hooks, MCP configs, supply chain).
- **Fine-tuning**
  - OpenAI is closing self-serve fine-tuning: no new jobs after Jan 6, 2027.
  - Google expanded Gemini SFT and RL tuning.
  - RL fine-tuning against graders.
- **Rounds**
  - AI-assisted rounds converged on one format: an AI assistant, an unfamiliar repo, and a bug, then a feature, then optimization.
  - Variants at Meta, Google and OpenAI.
  - AI interviewers.
  - Rounds that ban AI, and why.
- **Worked example:** the cost napkin math now uses 2026 mid-tier prices ($2/$10 per 1M tokens).

### Corrected
- **Sierra:** the earlier "no-AI debugging round" claim was wrong. Sierra's own post (Apr 22, 2026) describes these rounds:
  - a system-design phone screen
  - a Plan → Build (any AI tools) → Review onsite
  - a pilot round that improves a draft PR with coding agents
- **OpenAI:** the FDE / AI Deployment Engineer loop varies by team. A single 5-hour take-home is one undated account, not the standard.
- **"Temperature 0 = deterministic"** and **"LLM APIs are stateless"** are now qualified. Retired models (the o-series, Gemini 2.5) are no longer presented as current.
- **"Cross-encoder reranker"** wording updated to match current rerankers.

## 2026-07-12 — Round-by-Round Playbook

- Added [topic 11](./topics/11-rounds.md): decomposition, take-homes and video walkthroughs, AI-assisted and debugging rounds, and the discovery-call simulation.
- Mid-2026 updates across topics.

## 2026-07-02 — Initial release

- Topics 01–10, the interview question bank and the flashcards.
