---
title: "Karpathy: makemore and Neural Networks from Scratch"
source: "https://youtu.be/VMj-3S1tku0"
date: 2026-04-19
tags: [ai, neural-networks, karpathy, deep-learning, backpropagation]
---

# Karpathy: makemore and Neural Networks from Scratch

## Summary
Andrej Karpathy's "makemore" series is a foundational deep dive into the mechanics of neural networks. Starting from a blank Jupyter notebook, he builds `micrograd`—a tiny automatic differentiation engine— and then uses it to construct and train multi-layer perceptrons (MLPs). The core thesis is that neural networks are just mathematical expressions, and backpropagation is simply the chain rule from calculus applied recursively to those expressions.

## Key Insights

### 1. The Atomic Unit: The Value Object
At the heart of `micrograd` is a simple `Value` object that wraps a scalar. It tracks:
- **`data`**: The actual numerical value.
- **`grad`**: The local derivative (initially 0.0).
- **`_prev`**: A set of child nodes that created this value.
- **`_op`**: The operation that produced it (e.g., '+', '*').

This scalar-level approach is "excessive" for production but perfect for pedagogy. It breaks neural nets down into their atomic "atoms" of individual scalars, making the math transparent.

### 2. Backpropagation is Just the Chain Rule
Backpropagation isn't magic; it's just the **chain rule** applied recursively. 
- **Forward Pass:** We build the expression graph and compute the output.
- **Backward Pass:** We start at the output and move backward, applying the local derivative at each node to update the `grad` of its children. 
- **The Goal:** To find out how nudging each weight (or input) slightly in a positive direction would change the final loss.

### 3. Neural Networks are Mathematical Expressions
Karpathy emphasizes that a neural network is just a specific class of mathematical expression. It takes inputs (data) and weights, passes them through layers of neurons (which are just `sum(weights * inputs) + bias` followed by a non-linearity like `tanh`), and produces a loss. `micrograd` doesn't know anything about "neural nets"—it just knows how to differentiate math.

### 4. The "Makemore" Goal: Character-Level Modeling
The ultimate goal of the series is to build a model that can "make more" of something, starting with names. By training an MLP on a dataset of names, the network learns the statistical regularities of English (or any language) at the character level. It learns that 'q' is often followed by 'u', for example.

### 5. Efficiency vs. Understanding
While `micrograd` uses scalars, production libraries like PyTorch use **tensors** (n-dimensional arrays). The math doesn't change; tensors are just an efficiency hack to take advantage of parallelism on GPUs. Understanding the scalar version is the key to understanding the tensor version.

## Implications for The Agent Factory
For Synthweave and our agent development, this "from scratch" understanding is vital. We are building **harnesses** that wrap LLMs, but the underlying principles of **gradient-based learning** and **expression graphs** are the bedrock of AI. 

Understanding `micrograd` helps us see that:
- **Agents are Expression Graphs:** An agent's thought process is a forward pass through a complex graph of prompts and tools.
- **Optimization is Local:** Just like backprop, we improve agents by looking at the "local derivative" of their performance—what small change in the prompt or tool use yields a better outcome?
- **Substrate Matters:** Just as `micrograd` is the substrate for the neural net, our **Substrate** repo is the memory graph for our agents. We need to ensure our "backward pass" (learning from mistakes) is as robust as Karpathy's engine.

## Related
- [[harness-engineering]]
- [[cloudflare-workers-ai-edge-inference]]
- [[knowledge-graphs-as-agent-memory-substrate]]