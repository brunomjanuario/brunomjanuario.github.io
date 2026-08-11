---
title: "My AI Learning Roadmap"
date: 2026-08-11 10:00:00 +0000
categories: [AI Fundamentals]
tags: [roadmap, learning-plan]
---

I'm kicking off a ~28 week journey (about 7 months at 8-10 hrs/week) to go from
math foundations to building, fine-tuning, and deploying AI agents. Here's the
plan I'll be following and posting progress against.

Each week has a focus, resources, and a **checkpoint** — something I build or
prove, not just "watched a video."

## Phase 1: Math & Python Foundations (Weeks 1-3)

- **Week 1 — Linear Algebra**: vectors, matrices, dot products, eigenvalues.
  Checkpoint: implement matrix multiplication in raw Python, then NumPy, and
  compare speed.
- **Week 2 — Calculus & Probability**: derivatives, gradients, chain rule,
  basic distributions. Checkpoint: compute a gradient by hand, verify with
  sympy.
- **Week 3 — Python for ML**: NumPy, Pandas, Matplotlib. Checkpoint: load,
  clean, and plot a CSV dataset from memory, no tutorial open.

## Phase 2: Classical ML (Weeks 4-7)

- **Week 4 — Core Concepts**: supervised vs unsupervised learning, overfitting,
  bias-variance tradeoff.
- **Week 5 — Regression & Gradient Descent**: implement linear regression and
  gradient descent from scratch in NumPy.
- **Week 6 — Classification**: logistic regression, decision trees, random
  forests. Build a Titanic classifier from scratch, then compare to sklearn.
- **Week 7 — Consolidation Project**: full pipeline on a new dataset, solo,
  published to GitHub.

## Phase 3: Deep Learning Core (Weeks 8-13)

- **Week 8 — Neural Network Basics**: perceptrons, activation functions.
  Build a tiny autograd engine from scratch (Karpathy's micrograd).
- **Week 9 — Backpropagation**: train a 2-layer net using my own backprop code.
- **Week 10 — PyTorch Fundamentals**: rewrite the Week 9 network in idiomatic
  PyTorch.
- **Week 11 — CNNs**: train a CNN on CIFAR-10, target 70%+ test accuracy.
- **Week 12 — RNNs & Sequence Models**: train a character-level RNN to
  generate text in the style of a small corpus.
- **Week 13 — Consolidation Project**: write up backprop and gradient descent
  in my own words — teaching it is the real test of understanding.

## Phase 4: Transformers & LLM Architecture (Weeks 14-17)

- **Week 14 — Attention Mechanism**: implement single-head self-attention
  from scratch in PyTorch.
- **Week 15 — Full Transformer Architecture**: multi-head attention,
  positional encoding, layer norm, residual connections. Read "Attention Is
  All You Need."
- **Week 16 — Tokenization**: build a BPE tokenizer from scratch.
- **Week 17 — Consolidation**: combine tokenizer + transformer block into a
  minimal GPT architecture, confirm a forward pass runs end to end.

## Phase 5: Build Your Own Model From Scratch (Weeks 18-20)

- **Week 18 — nanoGPT Setup**: train nanoGPT on tiny Shakespeare, get
  coherent-ish text out.
- **Week 19 — Scaling Experiments**: train 2-3 variants with different sizes,
  document how output quality changes.
- **Week 20 — Custom Dataset**: train on a dataset I collect myself, write up
  what data quality does to output.

## Phase 6: Fine-Tuning (Weeks 21-23)

- **Week 21 — Fine-Tuning Fundamentals**: full fine-tuning vs PEFT, LoRA
  mechanics, quantization (QLoRA).
- **Week 22 — Hands-On LoRA Fine-Tuning**: LoRA fine-tune a small open model
  (Llama 3.2 1B/3B, Qwen2.5-1.5B, or Gemma 2B) on an instruction dataset.
- **Week 23 — Evaluation & Iteration**: build a before/after eval set and
  score the fine-tuned model against the base model.

## Phase 7: Agents in Code (Weeks 24-25)

- **Week 24 — Raw Tool-Calling Loop**: build an agent loop from scratch with
  2-3 real tools, no framework.
- **Week 25 — Multi-Step & Memory**: extend the agent to a multi-step task,
  then rebuild it with LangGraph and compare.

## Phase 8: Graph Engineering (Weeks 26-28)

- **Week 26 — Graph Theory Basics**: represent a dataset as a graph in
  NetworkX, run traversal/centrality queries.
- **Week 27 — GraphRAG**: build a small GraphRAG pipeline, find a multi-hop
  question where GraphRAG beats plain vector RAG.
- **Week 28 — Capstone**: combine everything — an agent querying a GraphRAG
  knowledge base, powered by a fine-tuned model. Full write-up with
  architecture diagram.

## Ongoing Habits

- **Public log**: commit every checkpoint to GitHub — this doubles as a
  portfolio.
- **No copy-paste rule**: after a resource, close it and reproduce the idea
  from memory.
- **Community**: r/MachineLearning, Hugging Face forums, EleutherAI Discord
  for when training loops fail silently.
- **Don't skip checkpoints** — they're the real signal of progress, not video
  completion.

That's the plan. Posts from here on will track progress week by week.
