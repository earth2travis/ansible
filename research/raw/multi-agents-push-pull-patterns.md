---
title: "Multi-Agents: The Push, The Pull, and The Practical Patterns"
source: "https://x.com/walden_yan/status/2047054401341370639"
date: 2026-04-23
tags: [multi-agent, context-engineering, agent-architecture, cognition, smart-friend]
---

# Multi-Agents: The Push, The Pull, and The Practical Patterns

## Summary
Walden Yan expands on the evolution of multi-agent systems, driven by a "push" from increased agent usage (creating management bottlenecks) and a "pull" from rising costs (necessitating cheaper, smarter architectures). The post details three practical patterns: the **Clean-Context Review Loop**, the **Smart Friend** escalation model, and **Higher-Level Delegation**.

## Key Insights

### 1. The "Clean-Context" Review Loop
Counterintuitively, review agents work best when they **do not share context** with the coding agent.
*   **Why:** Shared context leads to "Context Rot" (diminished attention/intelligence at long context lengths). A clean-context reviewer is forced to reason backward from the implementation, often catching logic errors and security vulnerabilities the original agent missed due to instruction bias or context overload.
*   **The Bridge:** The key is a "communication bridge" where the primary agent uses its broader context (user instructions, decisions) to filter the reviewer's feedback, preventing infinite loops or out-of-scope work.

### 2. The "Smart Friend" Pattern
To balance cost and capability, smaller/cheaper models can call out to larger/expensive "Smart Friends" for tricky sub-tasks.
*   **The Challenge:** Getting a "dumber" primary model to know *when* to escalate and *what* to ask is a training problem. 
*   **The Fix:** Instead of the primary model asking narrow questions, it should share a **fork of its full context** and ask broad questions ("What should I do?"). The Smart Friend should be "over-scoped," looking beyond the immediate question to suggest important guidance based on the agent's trajectory.
*   **Cross-Frontier Routing:** When using two frontier models (e.g., Claude vs. GPT), the pattern shifts from "difficulty escalation" to "capability routing" (e.g., using one for debugging, the other for visual reasoning).

### 3. Higher-Level Delegation (Map-Reduce-and-Manage)
For large scopes (e.g., a product feature spanning 10 PRs), a "Manager" agent breaks work into pieces for "Child" agents.
*   **The Friction:** Managers often default to being overly prescriptive because they lack the deep codebase context of the children. 
*   **The Solution:** Dedicated context engineering to ensure children surface discoveries that should change their siblings' work. The practical shape is **Map-Reduce-and-Manage**, not unstructured swarms.

### 4. The Core Through-Line
**"Writes stay single-threaded; additional agents contribute intelligence."**
Whether it's a clean-context reviewer, a smart friend, or a manager, the most robust systems avoid parallel writes. They use multiple agents to inject intelligence at every stage (planning, coding, review) while keeping the final decision-making and action-taking cohesive and single-threaded.

## Related
- [[single-threaded-write-pattern]]
- [[context-engineering-principles]]
- [[the-substrate-spec]]
- [[autogenesis-protocol]]