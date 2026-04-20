---
title: "The Micrograd Principle"
tags: [ai, learning, backpropagation, fundamentals]
related: [[karpathy-makemore-neural-networks-from-scratch]], [[harness-engineering]], [[the-context-stack-spec]]
source: research/raw/karpathy-makemore-neural-networks-from-scratch.md
---

# The Micrograd Principle

The **Micrograd Principle** states that **complexity is just efficiency hiding simplicity**. 

Andrej Karpathy's `micrograd` demonstrates that the entire power of modern deep learning—backpropagation, gradient descent, and neural network training—can be reduced to ~100 lines of Python code operating on individual scalars. 

## Why It Matters for Agents
In the context of The Agent Factory, this principle is a warning against "black box" thinking. 

1.  **Transparency:** Just as `micrograd` makes every derivative visible, our agent harnesses must make every "thought" and "tool call" visible in the **Substrate**. 
2.  **Local Optimization:** Backpropagation works because it calculates **local derivatives**. We don't need to understand the entire global state of an agent to improve it; we only need to understand how a small "nudge" in a prompt or a tool call affects the final outcome (the "loss").
3.  **The Substrate as the Engine:** If `micrograd` is the engine for neural nets, the **Substrate** (our shared knowledge graph) is the engine for our agents. It must be built on first principles—versioned, forkable, and mathematically sound—rather than just being a "bag of files."

## The "From Scratch" Ethos
We adopt the `micrograd` ethos: **Build it from scratch to understand it, then optimize it for scale.** We don't just use LLMs; we build the harnesses that make them useful. We don't just store data; we build the graph that makes it intelligent.