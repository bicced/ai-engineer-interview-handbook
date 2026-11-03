# Sources

These are the dated sources behind the handbook's factual claims, last checked **November 3, 2026**. Confidence levels:

- **Official**: the company or standards body says so.
- **Press**: reported by journalists.
- **Reported**: candidate write-ups or prep sites. Treat these as anecdotes.

Found something out of date? [Open an issue](./CONTRIBUTING.md) with a newer source.

## Market & salary

| Claim | Source | Date | Confidence |
|---|---|---|---|
| 7,300+ US FDE postings Jan–Sep 2026, vs ~1,200 in 2025 and under 200 in 2023; top posters Deloitte, Accenture, Google, KPMG | Lightcast data reported by CNBC | Oct 15, 2026 | Press |
| FDE postings up more than 1,000% year over year | Fortune | Sep 3, 2026 | Press |
| ~1,262 open FDE roles across 555 companies | Plank | Oct 18, 2026 | Press |
| Median advertised FDE salary ~$195K, up ~40% vs 2023 | Lightcast | Oct 2026 | Press |
| Anthropic FDE posting: MCP servers, sub-agents, agent skills, eval frameworks; 25–50% travel; London £225–255K; US base $280–320K | Anthropic careers postings | Sep 2026 | Official |
| AWS FDE organization (~$1B, "hundreds" of engineers) | Reuters; Business Insider | Jun 30 & Oct 4, 2026 | Press |
| Microsoft FDE as multidisciplinary pods | Microsoft "Command Line" post by its FDE CVP | Oct 28, 2026 | Official |
| OpenAI deployment company; "AI Deployment Engineer" title | OpenAI, "What FDE means at OpenAI"; CNBC | Oct 22 & Oct 15, 2026 | Official / Press |
| Claude Frontier Academy (graded deployment practical, badges) | Anthropic announcement | Oct 2, 2026 | Official |

## Company loops

| Claim | Source | Date | Confidence |
|---|---|---|---|
| Sierra: system-design phone screen; Plan → Build (any AI tools) → Review onsite; pilot round improving a draft PR with coding agents | Sierra engineering blog, "The AI-native interview" | Apr 22, 2026 | Official |
| Anthropic: no AI in interviews unless explicitly permitted | Anthropic candidate AI guidance | unchanged since Jul 2025 | Official |
| Anthropic round details (escalating coding, take-home brief, discovery simulation, eval-harness round) | Interview-prep sites | 2026 | Reported (low) |
| OpenAI FDE loop varies by team (take-home + walkthrough, AI-enabled Codex screen, or coding + design screens) | Candidate reports via prep sites (2–3) | late Oct 2026 | Reported (medium-low) |
| Google: Gemini-assisted code-comprehension round; "AI fluency is a requirement for all SWE jobs" | Business Insider | Oct 20, 2026 | Press |
| Meta AI-enabled round: bug fix → feature → optimize; verification graded | Meta candidate prep materials; candidate reports | 2026 | Official / Reported |
| Palantir: decomposition, re-engineering, learning rounds; AI reportedly banned | Candidate reports and guides | Jul–Aug 2026 | Reported (medium) |
| 46% of employers allow AI in technical interviews, 20% case by case, 34% ban | CoderPad survey | 2026 | Industry survey |
| HackerRank Chakra AI interviewer generally available | HackerRank | Oct 5, 2026 | Official |

## Models, APIs & agents

| Claim | Source | Date | Confidence |
|---|---|---|---|
| MCP 2026-07-28 spec: stateless, `input_required`, deprecations (roots, sampling, logging, HTTP+SSE, dynamic client registration), extensions | Model Context Protocol specification & SEPs | Jul 28, 2026 | Official |
| A2A and Agent Skills under the Linux Foundation's Agentic AI Foundation | Agentic AI Foundation announcements | Aug & Oct 22, 2026 | Official |
| OpenAI Assistants API shut down; Agent Builder and hosted Evals shut down Nov 30, 2026 | OpenAI deprecations page | Aug–Nov 2026 | Official |
| OpenAI self-serve fine-tuning: no new jobs after Jan 6, 2027 | OpenAI deprecations page | 2026 | Official |
| Gemini SFT GA; RL fine-tuning preview | Google Cloud release notes | Oct 19 & Sep 15, 2026 | Official |
| Sampling parameters rejected or ignored on several 2026 frontier models; prefill and forced tool choice rejected on some | Anthropic, OpenAI and Google API docs | Jul–Oct 2026 | Official |
| Prompt-cache writes billed (1.25×–2.5× by TTL) on OpenAI | OpenAI pricing | Oct 20, 2026 | Official |
| Agents use ~4× a chat's tokens; multi-agent systems ~15× (so ~4× a single agent) | Anthropic engineering, multi-agent research system post | Jun 2025 | Official |
| Agent harness CVEs (CI agents reading issues, git hooks, plugin pinning); LiteLLM PyPI compromise | Vendor advisories; Wiz; Cloud Security Alliance | Aug–Oct 2026 | Official / Press |

## Evals, security & compliance

| Claim | Source | Date | Confidence |
|---|---|---|---|
| Error analysis to saturation; hand-code ~30 traces; 100–200 labels per judge split into train/dev/test; TPR/TNR correction | Hamel Husain & Shreya Shankar, evals FAQ | Sep 18, 2026 | Practitioner |
| pass^k for agent reliability | Anthropic, "Demystifying evals for AI agents" | Jan 9, 2026 | Official |
| SWE-bench Verified retired as a headline metric | OpenAI | Feb 23, 2026 | Official |
| OpenTelemetry GenAI semantic conventions still "Development" | OpenTelemetry semantic-conventions-genai | Jun 2026 (v1.42.0) | Official |
| Tooling consolidation: Langfuse → ClickHouse, Arize → Dynatrace, Promptfoo → OpenAI | Company announcements | Jan–Oct 2026 | Official |
| OWASP LLM Top 10 2026 (prompt injection #1) | OWASP | Aug 3, 2026 | Official |
| EU AI Act Digital Omnibus: high-risk obligations moved to Dec 2, 2027 (Annex III) and Aug 2, 2028 (Annex I) | Regulation (EU) 2026/1744 | in force Jul 27, 2026 | Official |
| Colorado SB 26-189 replaces the Colorado AI Act, effective Jan 1, 2027 | Colorado General Assembly | 2026 | Official |
| Voice: speech-to-speech 825–1,780 ms vs cascaded ~958 ms median voice-to-voice | Coval benchmark | Oct 31, 2026 | Industry benchmark |
