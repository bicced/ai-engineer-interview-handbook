# RAG Interview Questions & Answers (Retrieval-Augmented Generation)

> Ground models in real, private, current data: chunking, embeddings, vector search, reranking, evaluation.

[◂ Back to the handbook](../README.md)

---

## Why RAG exists & when to use it
_~6 min_

**Retrieval-Augmented Generation** = fetch relevant information at query time and put it in the prompt, so the model answers from **your** data instead of its frozen training memory. It's the default architecture for 'chat with your docs', internal knowledge assistants, and any factual, source-grounded use case.

#### RAG solves three problems at once

- **Freshness**: inject data newer than the training cutoff.
- **Private knowledge**: use company/customer data the model never saw.
- **Hallucination + attribution**: ground answers in retrieved text and **cite sources**, so answers are checkable.

> 🎯 **RAG vs fine-tuning (classic question)** — **RAG** = give the model new **knowledge/facts** at runtime (cheap to update, attributable). **Fine-tuning** = change the model's **behavior/format/style** or teach a narrow skill. Need current facts? RAG. Need a consistent tone or output format the model won't follow via prompting? Fine-tune. Often you use **both**.

> 💡 **2026 nuance: retrieval moved up the stack** — The single-pass 'chunk → embed → top-k → stuff the prompt' pipeline is no longer the automatic default. In agentic systems, **retrieval is a tool the model calls** when it decides it needs something (**agentic RAG**). For navigable corpora like codebases, agents driving plain **grep/keyword search** famously held their own against vector indexes (Claude Code chose agentic search over embeddings) — but measurements since show **semantic search + grep together** beat grep alone (Cursor reported ~12.5% higher accuracy), especially for symptom-style queries ('the login page hangs'). The senior answer is **hybrid, exposed as tools** — and classic RAG for **large, fresh, permissioned**, high-QPS corpora. Asking **'do we even need a vector DB here?'** is still a hire signal; answering 'only grep, always' is not.

**◆ Lock it in**

- RAG injects **relevant data at query time** so the model answers from your sources.
- It fixes **freshness, private knowledge, and hallucination/attribution** together.
- RAG adds knowledge; **fine-tuning changes behavior** — different tools, often combined.
- 2026: **retrieval is a tool in the agent loop** — give agents **semantic + keyword search together**; classic RAG for big, fresh, permissioned, high-QPS corpora.

> 🧠 **Feynman drill** — A customer wants a bot that answers from their 10,000-page internal wiki, always with citations. RAG or fine-tune? Why?

**Retrieval check**

**Q: You need answers grounded in a constantly-changing internal knowledge base, with citations. Best approach?**

<details><summary>Show answer</summary>

**✓ RAG: retrieve the relevant chunks at query time and have the model answer from them with source citations.**

Changing facts + citations = RAG. Nightly fine-tuning is costly, slow to update, and gives no attribution. Pasting everything is expensive, slow, and hits lost-in-the-middle.
</details>

---

## The RAG pipeline end to end
_~7 min_

Every RAG system has two phases. Know them cold — this is a whiteboard staple.

#### Indexing (offline, once / on update)

1. **Load** source documents (PDFs, HTML, DB rows, tickets).
2. **Chunk** them into passages (e.g. 200–800 tokens) with some overlap.
3. **Embed** each chunk with an embedding model → vectors.
4. **Store** vectors (+ metadata + original text) in a **vector database**.

#### Retrieval + generation (online, per query)

1. **Embed the query** with the same embedding model.
2. **Search** the vector store for the top-k nearest chunks (often + keyword search = hybrid).
3. (Optional) **Rerank** the candidates with a reranker model for precision.
4. **Assemble** the prompt: instructions + retrieved chunks + user question.
5. **Generate** the answer, instructing the model to use only the context and **cite** chunks.

> 💡 **The whole game is retrieval quality** — If the right chunk isn't retrieved, the model can't answer — 'garbage in, garbage out.' Most RAG failures are **retrieval** failures, not generation failures. Spend your effort on chunking, hybrid search, and reranking.

**◆ Lock it in**

- Indexing: **load → chunk → embed → store**. Retrieval: **embed query → search → (rerank) → assemble → generate**.
- Use the **same embedding model** for chunks and queries.
- Most RAG failures are **retrieval** failures — invest there first.

> 🧠 **Feynman drill** — Draw the RAG pipeline from memory: name every step in both the indexing and query phases.

**Retrieval check**

**Q: Your RAG bot gives wrong answers. It turns out the correct passage was never in the retrieved set. This is primarily a failure of:**

<details><summary>Show answer</summary>

**✓ Retrieval — chunking/embedding/search didn't surface the right passage.**

If the right context isn't retrieved, generation can't recover. This is a retrieval-quality problem: revisit chunking, hybrid/keyword search, and reranking.
</details>

---

## Chunking & embeddings that don't sabotage you
_~6 min_

**Chunking** — how you split documents — quietly determines retrieval quality.

- **Too big**: a chunk covers many topics; embeddings get muddy; you waste context and money.
- **Too small**: chunks lose the surrounding meaning; answers get fragmented.
- **Overlap** (e.g. 10–20%) prevents cutting a sentence/idea across a boundary.
- **Structure-aware chunking** (by heading, section, or semantic boundary) beats naive fixed-size splits.
- Attach **metadata** (source, title, section, date, permissions) — you'll need it for filtering and citations.
- **Contextual chunk enrichment**: prepend a one-line, LLM-written description of the parent document/section to each chunk before embedding and keyword indexing ('This chunk is from the 2026 refund policy, section 4…'). Cheap at index time with prompt caching, and it rescues chunks that are ambiguous out of context.

> 💡 **Practical defaults** — Start ~400–600 tokens per chunk with ~15% overlap, split on semantic/heading boundaries, store rich metadata. Then **measure** and tune — don't guess forever.

> ⚠️ **Embedding mismatch** — You must embed queries and chunks with the **same model**. Changing embedding models means re-indexing **everything**. Choose deliberately and version it.

**◆ Lock it in**

- Chunk size is a tradeoff: too big = muddy, too small = fragmented; add **overlap**.
- Prefer **structure/semantic-aware** chunking and attach rich **metadata**.
- Query and chunk embeddings must use the **same model**; switching = full re-index.

> 🧠 **Feynman drill** — Explain why 4,000-token chunks might hurt retrieval accuracy even though they contain more info.

**Retrieval check**

**Q: Retrieval keeps pulling loosely-related chunks. A reasonable first tuning move is:**

<details><summary>Show answer</summary>

**✓ Reduce chunk size and split on semantic/heading boundaries with some overlap, then re-measure.**

Muddy retrieval often means chunks are too large or split mid-idea. Smaller, structure-aware chunks with overlap sharpen embeddings. Metadata helps (don't remove it); generation model/temperature aren't the issue.
</details>

---

## Better retrieval: hybrid search, reranking, and RAG failure modes
_~7 min_

Naive vector search is a baseline. Production RAG layers on precision techniques:

- **Hybrid search**: combine **semantic** (vector) with **keyword/BM25** search. Vectors catch meaning; keywords catch exact terms, IDs, and rare jargon vectors miss.
- **Reranking**: retrieve a broad top-50, then a **reranker** (a cross-encoder that reads query + chunk together) re-scores each and keeps the best 5. Big precision win for modest cost. 2026 rerankers also take **instructions** ('prefer the newest policy'), handle long inputs, and return **calibrated scores** — so a fixed threshold ('below X, say I don't know') actually works.
- **Query rewriting / expansion**: use an LLM to clarify or expand the query, or decompose multi-part questions before retrieval.
- **Metadata filtering**: restrict search by date, source, or **user permissions** (critical for access control).

#### The failure modes interviewers probe

- **Retrieved but ignored**: model has the context but answers from priors — instruct 'answer only from context; if absent, say you don't know.'
- **Right chunk missing**: fix chunking/hybrid/reranking.
- **Stale index**: source changed but you didn't re-index.
- **Conflicting sources**: retrieval returns contradictory chunks — need dedup/recency/authority handling.
- **Injection via documents**: a malicious doc contains 'ignore your instructions' — treat retrieved text as untrusted.

**◆ Lock it in**

- Layer **hybrid search + reranking** for precision; add **query rewriting** and **metadata/permission filters**.
- Instruct the model to answer **only from context** and admit ignorance.
- Know the failure modes: missing chunk, ignored context, stale index, conflicts, doc-borne injection.

> 🧠 **Feynman drill** — A RAG system misses answers when users search by exact part numbers. What technique fixes this and why?

**Retrieval check**

**Q: Users search by exact SKU codes and semantic vector search keeps missing them. Best fix?**

<details><summary>Show answer</summary>

**✓ Add keyword/BM25 search (hybrid) so exact tokens like SKUs are matched, alongside semantic search.**

Exact identifiers are a classic weakness of pure vector search. Hybrid (semantic + keyword/BM25) recovers exact-term matches like SKUs and IDs.
</details>

---

[◂ Back to the handbook](../README.md) · [▸ Practice this interactively](https://deployprep.vercel.app/track/fde)
