---
title: "My AI Learning Roadmap"
date: 2026-08-11 10:00:00 +0000
categories: [AI Fundamentals]
tags: [roadmap, learning-plan]
---

I'm kicking off a ~42 week journey (about 10 months at 8-10 hrs/week) to go from
math foundations to building, fine-tuning, and deploying AI agents. Here's the
plan I'll be following and posting progress against.

Each week has a focus, resources, and a **checkpoint** — something I build or
prove, not just "watched a video."

> **Update (2026-08-13):** I originally scoped this at 28 weeks, focused on the
> from-scratch path — build a transformer, train it, fine-tune it. After going
> through the [roadmap.sh AI Engineer track](https://roadmap.sh/ai-engineer) I
> realised it was missing the entire *shipping* layer: model APIs and token
> economics, prompt engineering, safety, embeddings, vector databases, RAG, and
> multimodal. Those are added below. Phase 1 is unchanged; everything after it
> got renumbered.
{: .prompt-info }

**Two tracks, braided together.** This merges the *from-scratch* path
(understand what's inside the model) with the *AI engineer* path (ship products
on top of one). The applied track starts early — Phase 2 — so I'm building
something real while the theory builds up, then it returns in depth from Phase 7
once I actually know what's in the box.

## Phase 1: Math & Python Foundations (Weeks 1-3)

- **Week 1 — Linear Algebra**: vectors, matrices, dot products, eigenvalues.
  Checkpoint: implement matrix multiplication in raw Python, then NumPy, and
  compare speed.
- **Week 2 — Calculus & Probability**: derivatives, gradients, chain rule,
  basic distributions. Checkpoint: compute a gradient by hand, verify with
  sympy.
- **Week 3 — Python for ML**: NumPy, Pandas, Matplotlib. Checkpoint: load,
  clean, and plot a CSV dataset from memory, no tutorial open.

## Phase 2: AI Engineering Quickstart (Weeks 4-6)

You can build genuinely useful things with pre-trained models before
understanding their internals. Doing this early should make the theory in
Phases 3-6 concrete instead of abstract.

- **Week 4 — LLM APIs & the Model Landscape**: AI engineer vs ML engineer,
  inference vs training, context lengths and knowledge cutoffs, the provider
  landscape, tokens and pricing. Checkpoint: a CLI tool that calls a model API,
  streams the response, counts tokens, and prints a running cost estimate —
  same prompt against two providers, compared on quality, latency, and cost.
- **Week 5 — Prompt Engineering**: system vs user prompts, few-shot,
  chain-of-thought, structured output, ReAct, sampling parameters. Checkpoint:
  4-5 prompt variants for one task plus a tiny eval harness that scores them
  over fixed inputs — and a write-up of which won and why.
- **Week 6 — AI Safety & Guardrails**: prompt injection (direct and indirect),
  jailbreaks, bias, privacy, moderation, constraining inputs and outputs.
  Checkpoint: try to break my own Week 5 app with 10 adversarial inputs, add
  defenses, re-run, document what got through before and after.

## Phase 3: Classical ML (Weeks 7-10)

- **Week 7 — Core Concepts**: supervised vs unsupervised learning, overfitting,
  bias-variance tradeoff.
- **Week 8 — Regression & Gradient Descent**: implement linear regression and
  gradient descent from scratch in NumPy.
- **Week 9 — Classification**: logistic regression, decision trees, random
  forests. Build a Titanic classifier from scratch, then compare to sklearn.
- **Week 10 — Consolidation Project**: full pipeline on a new dataset, solo,
  published to GitHub.

## Phase 4: Deep Learning Core (Weeks 11-16)

- **Week 11 — Neural Network Basics**: perceptrons, activation functions.
  Build a tiny autograd engine from scratch (Karpathy's micrograd).
- **Week 12 — Backpropagation**: train a 2-layer net using my own backprop code.
- **Week 13 — PyTorch Fundamentals**: rewrite the Week 12 network in idiomatic
  PyTorch.
- **Week 14 — CNNs**: train a CNN on CIFAR-10, target 70%+ test accuracy.
- **Week 15 — RNNs & Sequence Models**: train a character-level RNN to
  generate text in the style of a small corpus.
- **Week 16 — Consolidation Project**: write up backprop and gradient descent
  in my own words — teaching it is the real test of understanding.

## Phase 5: Transformers & LLM Architecture (Weeks 17-20)

- **Week 17 — Attention Mechanism**: implement single-head self-attention
  from scratch in PyTorch.
- **Week 18 — Full Transformer Architecture**: multi-head attention,
  positional encoding, layer norm, residual connections. Read "Attention Is
  All You Need."
- **Week 19 — Tokenization**: build a BPE tokenizer from scratch, then compare
  my token counts against a production tokenizer — closing the loop on the
  token accounting from Week 4.
- **Week 20 — Consolidation**: combine tokenizer + transformer block into a
  minimal GPT architecture, confirm a forward pass runs end to end.

## Phase 6: Build Your Own Model From Scratch (Weeks 21-23)

- **Week 21 — nanoGPT Setup**: train nanoGPT on tiny Shakespeare, get
  coherent-ish text out.
- **Week 22 — Scaling Experiments**: train 2-3 variants with different sizes,
  document how output quality changes.
- **Week 23 — Custom Dataset**: train on a dataset I collect myself, write up
  what data quality does to output.

## Phase 7: Open-Source Models & Local Inference (Weeks 24-25)

Having built a model, now learn the ecosystem of models other people built and
released. This is the bridge into fine-tuning — you can't fine-tune what you
can't load and run.

- **Week 24 — Hugging Face Ecosystem**: open vs closed weights and the
  licensing/cost/privacy trade-offs, the Hub, model cards, `transformers`
  pipelines, the Inference SDK. Checkpoint: run three open models of different
  sizes on one task, compare quality, speed, and memory.
- **Week 25 — Local Serving & Quantization**: Ollama, GGUF and 4/8-bit
  quantization, when local beats an API. Checkpoint: serve a quantized model
  locally and swap it into the Week 5 app behind the same interface, then
  benchmark cost-per-1000-calls against the hosted API.

## Phase 8: Fine-Tuning (Weeks 26-28)

- **Week 26 — Fine-Tuning Fundamentals**: full fine-tuning vs PEFT, LoRA
  mechanics, QLoRA — plus the decision framework for prompt vs RAG vs
  fine-tune. Fine-tuning teaches behavior and format; RAG supplies knowledge.
- **Week 27 — Hands-On LoRA Fine-Tuning**: LoRA fine-tune a small open model
  (Llama 3.2 1B/3B, Qwen2.5-1.5B, or Gemma 2B) on an instruction dataset.
- **Week 28 — Evaluation & Iteration**: score the fine-tuned model against the
  base model *and* against a well-prompted frontier model — including an honest
  account of where fine-tuning didn't help.

## Phase 9: Embeddings & Vector Search (Weeks 29-31)

- **Week 29 — What Embeddings Are**: dense representations of meaning, cosine
  similarity, and the link back to the token embeddings inside the transformer
  from Phase 5. Checkpoint: semantic search over ~500 texts with raw NumPy — no
  vector DB yet — plus a 2D visualisation of the embedding space.
- **Week 30 — Vector Databases**: what a vector DB adds over an array, index
  types (flat, IVF, HNSW), metadata filtering, hybrid keyword+vector search.
  Pick one (Chroma, Qdrant, pgvector…) and go deep. Checkpoint: move the corpus
  into it and benchmark recall and latency against the NumPy baseline.
- **Week 31 — Embeddings in Production**: hosted vs open embedding models,
  batching, caching, keeping an index fresh. Checkpoint: ship one non-RAG
  embedding product — a "related posts" recommender for this blog, or a
  duplicate detector — with a cost model.

## Phase 10: RAG (Weeks 32-34)

- **Week 32 — RAG From Scratch**: ingestion → chunking → embedding → retrieval
  → context assembly → generation, built with raw SDKs. Checkpoint: a RAG
  system over documents I care about that cites its chunks for every answer.
- **Week 33 — Making RAG Actually Work**: why naive RAG fails, query rewriting,
  re-ranking, parent-document retrieval, and — the part most tutorials skip —
  RAG evaluation: retrieval hit rate, faithfulness, answer relevance.
  Checkpoint: a 25-question eval set, a measured baseline, two improvements,
  and proof the numbers moved. Plus: plant a malicious instruction in an
  indexed document and show the Week 6 defenses hold.
- **Week 34 — Frameworks & Alternatives**: LangChain and LlamaIndex — what they
  abstract and what they hide. Read *after* my own version works, so nothing
  feels like magic. Checkpoint: rebuild Week 32 with a framework and give an
  honest verdict.

## Phase 11: Agents (Weeks 35-37)

- **Week 35 — Raw Tool-Calling Loop**: tool schema design, the call → parse →
  execute → feed back loop, ReAct in practice. Checkpoint: an agent loop from
  scratch with 2-3 real tools, no framework.
- **Week 36 — Multi-Step, Memory & Retrieval Tools**: state management, task
  decomposition, error handling, context window management, and handing the
  agent my Phase 10 RAG pipeline as a tool. Checkpoint: a multi-step task end
  to end, then the same thing in LangGraph, compared.
- **Week 37 — Agent Evaluation & Safety**: why agents fail (loops, wrong tool,
  hallucinated arguments, silent partial failure), tracing and observability,
  cost budgets per run, sandboxing and human-in-the-loop for irreversible
  actions. Checkpoint: full tracing, a 10-task eval suite, success rate and
  cost per task, then guardrails that measurably shrink the failure modes.

## Phase 12: Multimodal AI (Weeks 38-39)

- **Week 38 — Vision & Image Generation**: image understanding, document and
  chart extraction, image generation APIs, multimodal embeddings (CLIP).
  Checkpoint: extract line items from receipt photos into structured JSON,
  scored on field-level accuracy across 15 images.
- **Week 39 — Audio**: transcription (Whisper, hosted vs local), text-to-speech,
  latency for voice interfaces. Checkpoint: record → transcribe → run through
  my agent → speak the answer back, and find the latency bottleneck.

## Phase 13: Graph Engineering & Capstone (Weeks 40-42)

- **Week 40 — Graph Theory Basics**: represent a dataset as a graph in
  NetworkX, run traversal/centrality queries.
- **Week 41 — GraphRAG**: entity and relationship extraction with LLMs, graph
  retrieval vs vector retrieval. Checkpoint: build it over the *same* documents
  as Phase 10 so the comparison is direct, then find a multi-hop question where
  GraphRAG wins and verify it on the existing eval harness.
- **Week 42 — Capstone**: an agent querying both the vector RAG and the
  GraphRAG knowledge base, optionally powered by the fine-tuned model, with
  tracing, an eval suite, guardrails, and a cost model. Full write-up with
  architecture diagram and honest numbers.

## Ongoing Habits

- **Public log**: commit every checkpoint to GitHub — this doubles as a
  portfolio.
- **No copy-paste rule**: after a resource, close it and reproduce the idea
  from memory.
- **Evals before features**: from Phase 2 on, every AI feature gets a small
  eval set *before* I start improving it. "It feels better" is not a result.
  I think this is the habit that separates AI engineers from people who demo
  things.
- **Track cost and latency** on everything: tokens in, tokens out, dollars per
  run.
- **Safety isn't a phase**: Week 6 introduces it, but every later project
  assumes untrusted input and untrusted retrieved content.
- **Use AI dev tools deliberately**: great for plumbing, never for the
  checkpoint itself. If the model writes my backprop, I didn't learn backprop.
- **Community**: r/MachineLearning, Hugging Face forums, EleutherAI Discord,
  LocalLLaMA for when training loops fail silently.
- **Don't skip checkpoints** — they're the real signal of progress, not video
  completion.

That's the plan. Posts from here on will track progress week by week.
