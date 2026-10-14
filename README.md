# Awesome-LLM-Context-Memory-Optimization

# Top LLM Context Memory Optimization Ecosystem

**Curated List of SaaS Products & Open-Source GitHub Projects**  
*Focused on Persistent Agent Memory, Temporal Knowledge Graphs & Semantic Caching*  
**Last updated: March 2026**

This repository tracks notable **SaaS platforms** and **open-source projects** for **LLM Context Memory Optimization**. These tools extend effective context windows, reduce redundant token usage, and give agents durable recall — using vector search, temporal knowledge graphs, semantic caching, and KV compression to keep the right facts in prompt without flooding it.

**Examples** include Mem0, Zep, LangMem, Letta (MemGPT), Graphiti, Supermemory, Redis LangCache, and ICO-Cache (the category leaders).

**Open-source emphasis**: This section is heavily expanded with every major active project for self-hosting, custom memory architectures, and transparent retrieval — ideal for AI engineers prioritizing data sovereignty, developers building stateful agents, and teams avoiding per-memory cloud pricing.

Contributions welcome! Open a PR to add/update entries. Keep descriptions factual and link to official sites.

## Table of Contents
- [SaaS/Hosted Platforms](#saas-hosted-platforms)
- [Open-Source GitHub Projects](#open-source-github-projects)
- [How to Contribute](#how-to-contribute)
- [Disclaimer](#disclaimer)

## SaaS/Hosted Platforms

- **[Mem0 Cloud](https://mem0.ai/)**  
  Managed memory layer with 65k+ GitHub stars on the open-source core [citation:7]. Cloud write latency p95 ~800ms, search ~80ms, LOCOMO score 84.5% [citation:9]. Free tier: 100K memories, 10K API calls/month. Paid: $0.001/memory add, $0.0001/search. Enterprise $50K–$500K/year. **Self-hosted version requires Qdrant + Postgres + Neo4j** — non-trivial operational overhead [citation:9][citation:15].

- **[Zep Cloud](https://www.getzep.com/)**  
  Timeline-specialized memory service. Write latency p95 ~600ms, search ~50ms, LOCOMO 82.1% [citation:9]. **Cheaper than Mem0 at scale** ($0.0005/message vs $0.001) due to per-session rather than per-turn extraction. Free tier: 1K users, 10K messages/month. Enterprise $25K–$300K/year. Note: **Zep Community Edition is discontinued** — open-source efforts now focus solely on Graphiti [citation:8].

- **[Supermemory](https://supermemory.ai/)**  
  Consumer-facing "make AI remember you" product with **Profile API** and browser extension. Stores, indexes, and retrieves long-term context, RAG, and user profiles for AI apps. Provides **MCP server** so Claude, Cursor, and any MCP-compatible tool can directly call supermemory — a significant integration advantage in the current ecosystem [citation:2][citation:10].

- **[Graphlit](https://www.graphlit.com/)**  
  Cloud-native context layer that ingests documents, audio, video, and web data into semantic memory retrievable by agents via one API. Ranked among top managed memory services for agents [citation:10].

- **[LangMem](https://langchain-ai.github.io/langmem/)**  
  Managed memory service from LangChain for agents, providing extraction, storage, search, and summarization. Ranked alongside Graphlit as a strong managed option [citation:10].

## Open-Source GitHub Projects

- **[Mem0](https://github.com/mem0ai/mem0)**  
  **The most widely adopted open-source memory layer for AI agents** (65k+ stars, Apache 2.0) [citation:7]. Drop-in memory infrastructure: `pip install mem0ai`, call `Memory()`, and you have persistence in five lines. **Default components**: OpenAI gpt-5-mini for extraction, text-embedding-3-small for embeddings, local Qdrant for vectors, SQLite for history [citation:15]. **Self-hosted server** provides Docker stack with dashboard, per-user API keys, and request audit log. LOCOMO SOTA among open frameworks (84.5%). **Trade-offs**: extraction LLM is a hard dependency (offline operation awkward), graph store is a paid add-on for powerful retrieval, self-hosting requires Qdrant + Postgres + Neo4j [citation:9].

- **[Graphiti](https://github.com/getzep/graphiti)**  
  **Zep's open-source temporal knowledge graph framework** (31k+ stars, Apache 2.0) [citation:16]. Core innovation: **bi-temporal edge model** — each edge has `valid_at` (when fact was true in reality) and `created_at` (when written to graph) [citation:2]. When user says "I live in Beijing" then "I moved to Shanghai," the Beijing edge is marked expired, not deleted — historical reconstruction remains possible. **Cross-encoder reranking** provides fine-grained semantic matching beyond cosine similarity. **Neo4j dependency** is the main limitation — heavyweight compared to SQLite-based alternatives [citation:2]. Zep's commercial service is powered by Graphiti, ensuring continued engineering focus [citation:8].

- **[Letta (MemGPT)](https://github.com/letta-ai/letta)**  
  **Agent-native memory system with OS-inspired paging** [citation:17]. Agents "live indefinitely" with persistent memory stored in a database. **REST API server on port 8283** with full **Agent Development Environment (ADE)** web interface for creating, deploying, and observing agents [citation:6]. **Critical deployment note**: `pip install letta` defaults to SQLite, but **data migrations are not supported on SQLite** — for production, use PostgreSQL (`LETTA_PG_URI`) to ensure upgrade paths [citation:6]. ADE connects to self-hosted servers via REST API; agent data stays on your machine [citation:6].

- **[total-agent-memory](https://pypi.org/project/total-agent-memory/)**  
  **Full-stack local memory server for AI coding agents.** Features **6-stage recall pipeline**: BM25 (FTS5) + dense (FastEmbed) + fuzzy + graph expansion + optional CrossEncoder + MMR diversity [citation:1]. **3D WebGL knowledge graph viewer** handling 3,500+ nodes and 120,000+ edges. **A2A protocol** shares memory between multiple agents (backend + frontend + mobile in a team). **Phase-scoped rules** (`self_rules_context(phase="build")`) achieve **~70% token reduction** [citation:1]. Install via `npx -y total-agent-memory connect claude-code` or any MCP client. SQLite + FTS5 + FastEmbed stack — zero external dependencies beyond optional Ollama.

- **[ICO-Cache](https://pypi.org/project/ico-cache/)**  
  **Generalized LLM semantic cache with zero false hits.** Benchmarked **0% false-hit rate** on 100 challenging near-miss negatives (entity/quarter/topic swaps), **100% true-hit rate** on paraphrased queries, **~46ms hit latency** [citation:5]. **Multi-layered architecture**: exact match (SQLite) + semantic vector (LanceDB embedded) + centralized entity/topic/quarter metadata filtering. **Zero-infra embedded mode** (`pip install`, no Docker) or **server mode** (Qdrant + Redis for production). Concurrency guard tested at 20/50/100 simultaneous identical queries with **0 duplicate generations** [citation:5].

- **[deepcq](https://pypi.org/project/deepcq/)**  
  **Intent-aware memory querying without embeddings or vector stores.** Sits between agent memory and prompt — scores which sections are relevant to the current query and returns only those [citation:4]. **Five memory tiers**: AppState (live), SessionCache (bounded LRU), History (sliding window), LongTermStore (persistent), ReferenceData (lazy static). Relevance computed from query tokens via regex and lightweight stemmer — **no external NLP dependency** [citation:4]. `pip install deepcq`. Ideal for agents that need context reduction without the complexity of vector databases.

- **[GrayMatter](https://pkg.go.dev/github.com/angelnicolasc/graymatter)**  
  **Go-based memory engine with dramatic token reduction at scale.** Benchmark: **90% reduction at 100 sessions** (6,960 tokens → 670 tokens) [citation:19]. Seven memory tools via MCP. **Cross-client support**: Claude Code, Cursor, Codex, OpenCode, Antigravity, Windsurf, VS Code Copilot, Zed, Cline — 25 verified configs [citation:19]. `brew install`, `scoop install`, or pre-built binaries for Linux/macOS/Windows. Includes `graymatter demo` that seeds a store, runs consolidation, and opens a TUI with knowledge graph rendering [citation:19].

- **[NearLossless-Context](https://pypi.org/project/nearlossless-context/)**  
  **Training-free KV cache compression for long-context inference.** Keeps critical local neighborhoods while compressing the rest. **Sticky surface novelty** discovery achieves **~93% retrieval at 4k context** and **9/9 cells through 40k** with **~1k peak cache** — flat VRAM from 4k to 40k [citation:12]. Decode KV ~72 MB, peak VRAM ~8.3 GB flat. **Public LongBench**: full F1 0.28, novelty ~0.18 (honest gap vs internal suite) [citation:12]. Posthoc query-aware mode achieves **0.95–1.0× full F1** @ final 512–2048. For GPU inference pipelines needing context extension without retraining.

### Additional Strong Open-Source Options

- **MVR-cache** — ICML 2026 semantic caching with **multi-vector retrieval** and learned prompt segmentation. **37% higher cache hit rates** while maintaining correctness guarantees vs single-vector cosine similarity [citation:20].
- **TriCacheLLM-MMA** — Semantic Redis HNSW cache with async Python APIs, metadata filtering (user, time, document version), and embedded ChromaDB option [citation:13].
- **nautilus-compass** — MCP-native memory with **drift detection** (AUC 0.83 on held-out failure transcripts), Merkle integrity audit log, and LongMemEval-S retrieval **P@1 0.890** vs Mem0's 0.774 [citation:3][citation:11].
- **Redis LangCache** — Redis-based dual-tier memory (working + long-term semantic) with semantic caching for agents [citation:10].
- **Recall.ai** — Meeting and conversation memory extraction for agents.

**Frameworks for building custom systems**: Combine **Mem0** for drop-in extraction and multi-scope memory (user_id, agent_id, run_id), **Graphiti** for temporal relationship reasoning where fact evolution matters, **Letta** for OS-paged agent memory with production PostgreSQL, **total-agent-memory** for coding agents needing 6-stage hybrid recall, **ICO-Cache** for zero-false-hit semantic caching, and **GrayMatter** for cross-client token reduction at scale to build a complete self-hosted context memory stack.

## How to Contribute

1. Fork the repo.
2. Add/edit entries in `README.md` (follow existing format).
3. Include: name, link, 1–2 sentence description, and whether it's SaaS or open-source.
4. Submit PR with a short explanation.

Star the repo if you find it useful!

## Disclaimer

- This is a **community-curated** list — not exhaustive and not an endorsement.
- LLM context memory tools must comply with data protection regulations (GDPR, CCPA) and handle user memory with appropriate consent and deletion mechanisms. **Memory extraction quality depends on the underlying LLM** — default models like gpt-5-mini may hallucinate or over-extract [citation:9]. **Self-hosting operational costs vary significantly**: Mem0 requires Qdrant + Postgres + Neo4j, Graphiti requires Neo4j, Letta requires PostgreSQL for production data migrations [citation:6][citation:9].
- Self-hosted open-source solutions require proper infrastructure planning, database migrations, and monitoring for retrieval degradation.

---

**Made for AI engineers, agent developers, platform teams, and developers building persistent memory systems for LLMs.**  
Let's make LLM context memory optimization more open, efficient, and production-ready.
