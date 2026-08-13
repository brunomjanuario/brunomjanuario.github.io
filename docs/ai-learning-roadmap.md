# AI Learning Roadmap: Zero to Building, Fine-Tuning & Deploying Agents

**Pace assumption:** 8-10 hrs/week. Adjust timelines proportionally if you have more or less time.
**Total duration:** ~42 weeks (~10 months)

Each week has: a focus, resources, and a checkpoint (something you build/prove, not just "watched a video").

**Two tracks, braided together.** This roadmap merges the *from-scratch/research* path (build a transformer, train it, fine-tune it) with the *AI engineer* path (ship products on top of models: APIs, RAG, agents, safety). The applied track starts early — Phase 2 — so you're shipping something real while the theory builds up, then returns in depth from Phase 7 onward once you understand what's inside the box.

---

## PHASE 1: Math & Python Foundations (Weeks 1-3)

### Week 1 — Linear Algebra
- **Learn:** vectors, matrices, dot products, matrix multiplication, eigenvalues/eigenvectors (conceptually)
- **Resources:** 3Blue1Brown "Essence of Linear Algebra" (YouTube playlist, ~3 hrs total)
- **Checkpoint:** Implement matrix multiplication in raw Python (no NumPy), then redo it in NumPy and compare speed.

### Week 2 — Calculus & Probability
- **Learn:** derivatives, partial derivatives, gradients, chain rule; basic probability distributions, mean/variance
- **Resources:** 3Blue1Brown "Essence of Calculus"; Khan Academy Statistics & Probability (skim, don't complete fully)
- **Checkpoint:** Compute the gradient of a simple multivariable function by hand, then verify with a symbolic library (sympy).

### Week 3 — Python for ML
- **Learn:** NumPy (arrays, broadcasting, vectorization), Pandas basics, Matplotlib
- **Resources:** NumPy official quickstart docs; freeCodeCamp "NumPy for Data Science" video
- **Checkpoint:** Load a CSV dataset, clean it, and plot two variables against each other — no tutorial open, from memory.

---

## PHASE 2: AI Engineering Quickstart (Weeks 4-6)

*Why here:* you can build genuinely useful things with pre-trained models before you understand their internals. This phase gives you working intuition (and a shipped project) that makes the theory in Phases 3-6 concrete instead of abstract.

### Week 4 — LLM APIs & the Model Landscape
- **Learn:** what an AI Engineer is vs an ML Engineer (building *with* models vs building models); inference vs training; pre-trained models — benefits and limitations; the provider landscape (Anthropic Claude, OpenAI, Google Gemini, Mistral, Cohere) and hosting platforms (Azure AI, AWS Bedrock/SageMaker, Hugging Face); context length, knowledge cutoffs, capability tiers; tokens — tokenization from the *user's* side, token counting, max tokens, pricing per input/output token
- **Resources:** Anthropic API docs (Messages API); OpenAI Chat Completions docs; roadmap.sh AI Engineer roadmap (use as a checklist, not a curriculum)
- **Checkpoint:** Build a small CLI tool that calls a model API, streams the response, counts tokens for every call, and prints a running cost estimate. Run the same prompt against two providers and compare output, latency, and cost.

### Week 5 — Prompt Engineering
- **Learn:** system vs user prompts, zero-shot / few-shot, chain-of-thought, structured output (JSON schemas / tool-shaped responses), ReAct prompting (reason → act → observe), temperature and sampling parameters, prompt versioning
- **Resources:** Anthropic prompt engineering guide; roadmap.sh Prompt Engineering roadmap; OpenAI playground for fast iteration
- **Checkpoint:** Pick one task (e.g. extracting structured data from messy text). Write 4-5 prompt variants, build a tiny eval harness that runs all of them over 10-15 fixed inputs and scores accuracy. Document which variant wins and *why*.

### Week 6 — AI Safety, Ethics & Guardrails
- **Learn:** prompt injection attacks (direct and indirect, e.g. instructions hidden in retrieved documents), jailbreaks, bias and fairness, security and privacy concerns (never put secrets or PII in prompts/logs), adversarial testing, moderation APIs, constraining inputs and outputs, attaching end-user IDs for abuse tracing, safety best practices
- **Resources:** OpenAI safety best practices + Moderation API docs; Anthropic docs on prompt injection and safe tool use; OWASP Top 10 for LLM Applications
- **Checkpoint:** Try to break your own Week 5 app — write 10 adversarial inputs (injection, PII extraction, off-task abuse). Add defenses (input validation, output constraints, a moderation pass, an untrusted-content boundary in the prompt) and re-run. Document what got through before and after.

---

## PHASE 3: Classical ML (Weeks 7-10)

### Week 7 — Core Concepts
- **Learn:** supervised vs unsupervised learning, train/test split, overfitting, bias-variance tradeoff
- **Resources:** Andrew Ng's Machine Learning Specialization (Coursera) — Course 1, weeks 1-2
- **Checkpoint:** Explain overfitting to someone (or write it out) using a concrete example, no jargon.

### Week 8 — Regression & Gradient Descent
- **Learn:** linear regression, cost functions, gradient descent mechanics
- **Resources:** ML Specialization Course 1, weeks 2-3
- **Checkpoint:** Implement linear regression + gradient descent from scratch in NumPy (no sklearn). Fit it to a toy dataset.

### Week 9 — Classification
- **Learn:** logistic regression, decision trees, random forests, evaluation metrics (precision/recall/F1/ROC)
- **Resources:** ML Specialization Course 1, week 3 + Course 2 skim
- **Checkpoint:** Build a classifier on the Titanic dataset (Kaggle) — implement logistic regression yourself, then compare to sklearn's.

### Week 10 — Consolidation Project
- **Checkpoint:** Pick one dataset (not Titanic) and do the full pipeline solo: clean data → engineer features → train 2 models → evaluate → write a 1-page summary of what worked and why. Put it on GitHub.

---

## PHASE 4: Deep Learning Core (Weeks 11-16)

### Week 11 — Neural Network Basics
- **Learn:** perceptrons, activation functions (ReLU, sigmoid, softmax), forward pass
- **Resources:** Andrej Karpathy "Neural Networks: Zero to Hero" — Video 1 (micrograd)
- **Checkpoint:** Build a tiny autograd engine from scratch (following Karpathy but typing every line yourself, not pasting).

### Week 12 — Backpropagation
- **Learn:** chain rule applied to networks, computing gradients, backprop algorithm
- **Resources:** Karpathy Zero to Hero — Video 2 (makemore, part 1)
- **Checkpoint:** Train a 2-layer neural net on a toy classification problem using only your own backprop code.

### Week 13 — PyTorch Fundamentals
- **Learn:** tensors, autograd, `nn.Module`, optimizers, training loops in PyTorch
- **Resources:** PyTorch official "Learn the Basics" tutorial; rewrite your Week 12 network in PyTorch
- **Checkpoint:** Same toy problem, now in idiomatic PyTorch. Confirm it matches your from-scratch results.

### Week 14 — CNNs
- **Learn:** convolutions, pooling, image classification architectures
- **Resources:** Fast.ai Practical Deep Learning, Lesson 1-2 (or CS231n notes if you want more rigor)
- **Checkpoint:** Train a CNN on CIFAR-10, get it above 70% test accuracy.

### Week 15 — RNNs & Sequence Models
- **Learn:** why sequences need different architectures, RNN/LSTM basics, vanishing gradients
- **Resources:** Karpathy Zero to Hero — Videos 3-5 (makemore parts 2-4, builds toward language modeling)
- **Checkpoint:** Train a character-level RNN to generate text in the style of a small corpus you choose (song lyrics, your own writing, etc.)

### Week 16 — Consolidation Project
- **Checkpoint:** Write a short document (or blog post) explaining backprop and gradient descent in your own words with a diagram. Teaching it is the real test of whether you understand it.

---

## PHASE 5: Transformers & LLM Architecture (Weeks 17-20)

### Week 17 — Attention Mechanism
- **Learn:** self-attention, query/key/value, why attention beats RNNs for long sequences
- **Resources:** Karpathy "Let's build GPT from scratch" (YouTube, ~2 hrs) — first half
- **Checkpoint:** Implement single-head self-attention from scratch in PyTorch, verify shapes at every step.

### Week 18 — Full Transformer Architecture
- **Learn:** multi-head attention, positional encoding, layer norm, feedforward blocks, residual connections
- **Resources:** Karpathy GPT video — second half; read "Attention Is All You Need" now that the pieces are familiar
- **Checkpoint:** Assemble a full transformer block from your attention implementation.

### Week 19 — Tokenization
- **Learn:** byte-pair encoding (BPE), vocabulary construction, why tokenization matters for model behavior — and connect it back to the token counting and pricing you did in Week 4
- **Resources:** Karpathy "Let's build the GPT Tokenizer" (YouTube)
- **Checkpoint:** Build a simple BPE tokenizer from scratch and tokenize a paragraph of text. Compare your token counts against a production tokenizer on the same text and explain the differences.

### Week 20 — Consolidation
- **Checkpoint:** Combine tokenizer + transformer block into a minimal GPT architecture (no training yet) — confirm a forward pass runs end to end on dummy input.

---

## PHASE 6: Build Your Own Model From Scratch (Weeks 21-23)

### Week 21 — nanoGPT Setup
- **Learn:** training loop mechanics, loss curves, learning rate scheduling, batching for language models
- **Resources:** Karpathy's `nanoGPT` GitHub repo — read the code fully before running it
- **Checkpoint:** Train nanoGPT on the tiny Shakespeare dataset on your own GPU (or a rented cloud GPU / Colab). Get coherent-ish text out.

### Week 22 — Scaling Experiments
- **Learn:** how model size, context length, and data size affect output quality; overfitting in language models
- **Resources:** Your own nanoGPT setup — vary hyperparameters
- **Checkpoint:** Train 2-3 variants (different layer counts / embedding sizes) and document how output quality changes. This is where "understanding AI under the hood" becomes tangible.

### Week 23 — Custom Dataset
- **Checkpoint:** Train your small GPT on a dataset you collected yourself (your own writing, a niche text corpus, song lyrics, etc.). Write up what you learned about data quality's effect on output.

---

## PHASE 7: Open-Source Models & Local Inference (Weeks 24-25)

*New phase.* You've built a model; now learn the ecosystem of models other people built and released. This is the bridge into fine-tuning — you can't fine-tune what you can't load and run.

### Week 24 — Hugging Face Ecosystem
- **Learn:** open vs closed source models (weights, licenses, cost, privacy, control trade-offs); the Hugging Face Hub — model cards, tasks, datasets, Spaces; `transformers` pipelines; the Inference SDK / Inference Endpoints; how to actually *choose* an open model for a task (size vs quality vs license)
- **Resources:** Hugging Face Hub docs + `transformers` quickstart; Hugging Face "Tasks" pages
- **Checkpoint:** Pick three open models of different sizes for one task (e.g. summarization). Run all three locally or via the Inference SDK, and write a comparison of quality, speed, and memory footprint.

### Week 25 — Local Serving & Quantization
- **Learn:** running models on your own hardware with Ollama (models, Modelfiles, the SDK/REST API); quantization formats (GGUF, 4-bit/8-bit) and their quality trade-off; when local beats an API (privacy, cost at volume, offline, latency) and when it doesn't; browser-side inference with Transformers.js
- **Resources:** Ollama docs + model library; llama.cpp / GGUF quantization notes
- **Checkpoint:** Serve a quantized open model locally with Ollama and swap it into your Week 5 app behind the same interface. Benchmark it against the hosted API on quality, latency, and cost-per-1000-calls, and write up when you'd choose each.

---

## PHASE 8: Fine-Tuning (Weeks 26-28)

### Week 26 — Fine-Tuning Fundamentals
- **Learn:** full fine-tuning vs parameter-efficient fine-tuning, LoRA mechanics, quantization (QLoRA); **when to fine-tune vs when to prompt-engineer vs when to use RAG** (fine-tuning teaches behavior/format/style; RAG supplies knowledge — get this decision framework straight now)
- **Resources:** Hugging Face PEFT documentation; Hugging Face "LLM Course" chapter on fine-tuning
- **Checkpoint:** Read and annotate the LoRA paper's core idea (low-rank decomposition) — write a plain-English explanation. Then write a one-page decision guide: given a problem, prompt / RAG / fine-tune / combination?

### Week 27 — Hands-On LoRA Fine-Tuning
- **Learn:** dataset formatting for instruction tuning, using `transformers` + `peft` + `trl`
- **Resources:** Hugging Face `trl` library examples; pick a small open model (Llama 3.2 1B/3B, Qwen2.5-1.5B, or Gemma 2B)
- **Checkpoint:** LoRA fine-tune your chosen model on a small instruction dataset (or one you build yourself) on Colab or a consumer GPU. Compare outputs before/after.

### Week 28 — Evaluation & Iteration
- **Learn:** how to evaluate fine-tuned models (perplexity, task-specific metrics, LLM-as-judge, human eval); also learn hosted fine-tuning APIs (OpenAI/Anthropic-style) and how they compare to doing it yourself
- **Checkpoint:** Build a before/after eval set (20-30 prompts) and score your fine-tuned model against the base model *and* against a well-prompted frontier model. Document results honestly, including where fine-tuning didn't help.

---

## PHASE 9: Embeddings & Vector Search (Weeks 29-31)

*New phase.* The foundation under RAG, semantic search, and most retrieval-shaped products.

### Week 29 — What Embeddings Are
- **Learn:** embeddings as dense vector representations of meaning; why cosine similarity works; embedding dimensions; the connection to the token embeddings inside the transformer you built in Phase 5; use cases — semantic search, recommendation systems, anomaly detection, data classification, clustering, deduplication
- **Resources:** OpenAI Embeddings API docs (models, dimensions, pricing); `sentence-transformers` documentation; MTEB leaderboard on Hugging Face for choosing a model
- **Checkpoint:** Embed ~500 short texts (your notes, blog posts, or a public dataset). Build semantic search over them with raw NumPy cosine similarity — no vector DB yet. Then visualize the embedding space in 2D (t-SNE or UMAP) and confirm the clusters make sense.

### Week 30 — Vector Databases
- **Learn:** what a vector DB adds over a NumPy array (persistence, ANN indexes, filtering, scale); index types (flat, IVF, HNSW) and the recall/speed trade-off; metadata filtering and hybrid search (vector + keyword/BM25); the landscape — Chroma, Qdrant, Weaviate, FAISS, LanceDB, Pinecone, pgvector/Supabase, MongoDB Atlas — **pick one and go deep**
- **Resources:** Docs for your chosen DB (Chroma or Qdrant are the easiest starts; pgvector if you already like Postgres)
- **Checkpoint:** Move your Week 29 corpus into a real vector DB. Implement similarity search with metadata filtering, then add hybrid keyword+vector search. Benchmark recall and latency against your NumPy baseline.

### Week 31 — Embeddings in Production
- **Learn:** open-source vs hosted embedding models (cost, privacy, dimension size, re-embedding cost when you switch models); batching and rate limits; caching; keeping an index fresh as documents change
- **Checkpoint:** Build one non-RAG embedding product end to end — e.g. a "related posts" recommender for your blog, a duplicate-issue detector, or an anomaly detector over log messages. Ship it and write up the cost model.

---

## PHASE 10: RAG (Weeks 32-34)

*New phase.* The single most common thing AI engineers actually get paid to build.

### Week 32 — RAG From Scratch
- **Learn:** the full pipeline — ingestion → chunking → embedding → vector store → retrieval → context assembly → generation; chunking strategies (fixed, sentence, semantic, parent-document) and why chunk size dominates quality; prompt construction with retrieved context and citations
- **Resources:** Build it with raw SDKs first — do *not* start with a framework
- **Checkpoint:** Build a RAG system over a document set you care about (docs, papers, your own notes), using only the model SDK plus your Phase 9 vector DB. It must cite which chunks it used for every answer.

### Week 33 — Making RAG Actually Work
- **Learn:** why naive RAG fails (bad chunking, lost context, retrieval misses, irrelevant top-k, contradictory sources); improvements — query rewriting, HyDE, re-ranking, MMR for diversity, parent-document retrieval, metadata filters; **RAG evaluation** (retrieval hit rate, faithfulness/groundedness, answer relevance) and how to detect hallucination against retrieved context; indirect prompt injection through retrieved documents (tie back to Week 6)
- **Resources:** RAGAS or a similar eval library; re-ranker models on Hugging Face
- **Checkpoint:** Build a 25-question eval set for your Week 32 system with known correct answers. Measure baseline retrieval and answer quality, apply two improvements, and prove the numbers moved. Also plant a malicious instruction inside one indexed document and show your defenses hold.

### Week 34 — Frameworks & Alternatives
- **Learn:** LangChain and LlamaIndex — what they abstract, what they hide, when the abstraction earns its keep; hosted/managed retrieval options and their trade-offs (lock-in, cost, control)
- **Resources:** LangChain and LlamaIndex docs — read *after* your own implementation works, so nothing feels like magic
- **Checkpoint:** Rebuild your Week 32 RAG system with LlamaIndex or LangChain. Compare line count, flexibility, debuggability, and performance, and write an honest verdict on when you'd use each.

---

## PHASE 11: Agents (Weeks 35-37)

### Week 35 — Raw Tool-Calling Loop
- **Learn:** function/tool calling via the Anthropic or OpenAI API, tool schema design, the agent loop pattern (call → parse → execute → feed back), ReAct prompting in practice, why tool descriptions matter as much as code
- **Resources:** Anthropic API docs on tool use; OpenAI function calling docs
- **Checkpoint:** Build an agent loop from scratch in Python with 2-3 real tools (e.g. web search, calculator, file read/write) — no framework, just the raw API and a while loop.

### Week 36 — Multi-Step, Memory & Retrieval Tools
- **Learn:** conversation state management, multi-step task decomposition, error handling and retries in agent loops, context window management (summarization, trimming), giving the agent your Phase 10 RAG pipeline as a tool, agents vs RAG vs plain prompting as architectural choices
- **Resources:** LangGraph docs (read after you've built your own loop); Anthropic docs on agent patterns
- **Checkpoint:** Extend your agent to handle a multi-step task ("research X, then write a summary file") end to end with your RAG system as one of its tools. Then rebuild the same thing in LangGraph and compare the code.

### Week 37 — Agent Evaluation & Safety
- **Learn:** why agents fail (loops, wrong tool, hallucinated arguments, silent partial failure), tracing and observability (LangSmith, LangFuse, or OpenTelemetry-style logging), cost/latency budgets per run, agent-specific safety — sandboxing tool execution, human-in-the-loop confirmation for irreversible actions, injection through tool outputs
- **Checkpoint:** Add tracing to your agent so every run logs steps, tool calls, tokens, and cost. Build a 10-task eval suite, measure success rate and cost per task, then add guardrails (confirmation on destructive tools, step limits, timeout) and prove failure modes shrank.

---

## PHASE 12: Multimodal AI (Weeks 38-39)

*New phase.* Text-only is a shrinking share of what these systems do.

### Week 38 — Vision & Image Generation
- **Learn:** image understanding via vision APIs (Claude/GPT/Gemini vision), document and chart extraction, OCR-style tasks; image generation APIs (DALL·E, Stable Diffusion via Hugging Face or Replicate); video understanding basics; multimodal embeddings (CLIP) and image similarity search
- **Resources:** Vision API docs for your chosen provider; Hugging Face multimodal task pages; Replicate for hosted open models
- **Checkpoint:** Build something that takes an image and produces structured output — e.g. extract line items from a receipt or invoice photo into JSON, with an eval set of 15 images scoring field-level accuracy.

### Week 39 — Audio: Speech-to-Text & Text-to-Speech
- **Learn:** transcription (Whisper — hosted API vs local), diarization basics, text-to-speech, latency considerations for voice interfaces, chaining audio → LLM → audio
- **Resources:** OpenAI Whisper API + open-source Whisper; a TTS API or open model
- **Checkpoint:** Build a voice pipeline: record audio → transcribe → run through your agent or RAG system → speak the answer back. Measure end-to-end latency and identify the bottleneck.

---

## PHASE 13: Graph Engineering & Capstone (Weeks 40-42)

### Week 40 — Graph Theory Basics
- **Learn:** nodes, edges, directed/undirected graphs, traversal algorithms, centrality measures
- **Resources:** NetworkX documentation + tutorial
- **Checkpoint:** Represent a small dataset (e.g. your own contact network, or a set of Wikipedia articles) as a graph in NetworkX and run basic traversal/centrality queries.

### Week 41 — GraphRAG
- **Learn:** entity/relationship extraction with LLMs, building a knowledge graph from unstructured text, graph-based retrieval vs vector retrieval (and hybrids), community detection for global summarization
- **Resources:** Microsoft's GraphRAG paper/repo (as reference, not to copy wholesale); Neo4j's GraphRAG tutorials
- **Checkpoint:** Build a small GraphRAG pipeline over the same document set as Phase 10, so you can compare directly. Design one multi-hop question that your plain vector RAG answers wrong and GraphRAG answers right — verify it on your existing eval harness.

### Week 42 — Capstone
- **Checkpoint:** Combine everything: an agent (your own tool-calling loop or LangGraph) that queries both your vector RAG and your GraphRAG knowledge base, optionally powered by a model you've fine-tuned, with tracing, an eval suite, guardrails, and a cost model. Write it up with an architecture diagram, honest numbers, what worked, and what didn't.

---

## Ongoing Habits Throughout
- **Public log:** commit every checkpoint to a GitHub repo. This becomes your portfolio automatically.
- **No copy-paste rule:** after watching/reading a resource, close it and reproduce the idea from memory. Struggling here is where learning happens.
- **Evals before features.** From Phase 2 onward, every AI feature you build gets a small eval set *before* you start improving it. "It feels better" is not a result. This is the single habit that separates AI engineers from people who demo things.
- **Track cost and latency** on everything you ship. Tokens in, tokens out, dollars per run.
- **Safety is not a phase.** Week 6 introduces it; every later project should assume untrusted input and untrusted retrieved content.
- **Use AI dev tools deliberately** (Claude Code, Cursor, Copilot). Use them to move faster on plumbing, but never for the checkpoint itself — if the model writes your backprop, you didn't learn backprop.
- **Community:** r/MachineLearning, Hugging Face forums, EleutherAI Discord, LocalLLaMA — use when training loops fail silently or loss won't converge (this happens constantly, it's normal).
- **Don't skip checkpoints.** They're the actual signal of progress, not video completion.

## If You Have Less Time
Compress by merging weeks rather than skipping checkpoints — e.g. combine Weeks 1-2 if your math background is already decent.

Two sensible reductions:
- **Applied fast track (~20 weeks):** Phases 1, 2, 7, 9, 10, 11, 12. Skip classical ML, deep learning core, transformers-from-scratch, and fine-tuning. You'll be employable as an AI engineer sooner but won't understand what's inside the model — plan to come back to Phases 3-6.
- **Research-first track (~26 weeks):** Phases 1, 3, 4, 5, 6, 8, plus Phase 11. This is close to the original roadmap. Deeper understanding, weaker product skills.

The full 42-week path is the one that produces someone who can both build a transformer and ship a product on top of one — go in the order above unless time forces a cut.

---

## What Changed & Why (merge notes)

This roadmap was extended using the roadmap.sh **AI Engineer** track. Phase 1 is unchanged. Everything after it was renumbered, and these blocks were added:

| Added | Where | Source topics from the AI Engineer roadmap |
|---|---|---|
| AI Engineering Quickstart | Phase 2 (W4-6) | AI Engineer vs ML Engineer, pre-trained models, model landscape, OpenAI/Anthropic APIs, token management & pricing, prompt engineering, AI safety & ethics, moderation |
| Open-Source Models & Local Inference | Phase 7 (W24-25) | Open vs closed source, Hugging Face Hub/Tasks/Inference SDK, Ollama, Transformers.js, quantization |
| Embeddings & Vector Search | Phase 9 (W29-31) | What are embeddings, semantic search, recommendations, anomaly detection, classification, embedding APIs, sentence-transformers, vector DBs, indexing, similarity search |
| RAG | Phase 10 (W32-34) | RAG use cases, RAG vs fine-tuning, chunking, retrieval, generation, LangChain, LlamaIndex |
| Multimodal AI | Phase 12 (W38-39) | Image understanding/generation, video understanding, Vision API, DALL·E, Whisper, TTS/STT |
| Agent evaluation & safety | Phase 11 W37 (new week) | AI agents, ReAct, tools/functions, plus observability the source roadmap under-covers |
| Dev tools, eval habit, cost tracking | Ongoing Habits | AI code editors, code completion tools |

Deliberate deviations from the source roadmap: it is OpenAI-centric and framework-first — this version stays provider-agnostic, and always builds the raw version before introducing LangChain/LlamaIndex/LangGraph. It also has no evaluation story at all, which is the biggest gap; evals are added throughout.
