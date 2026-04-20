---
title: "Factory Architecture: Cloudflare-First Specification"
date: 2026-04-20
status: "draft"
version: "1.0"
---

# Factory Architecture: Cloudflare-First Specification

## 1. Overview
This document defines the technical architecture for **The Agent Factory**—the shared operational environment for [[Sivart]], [[Koda]], and [[Zookooree]]. It leverages Cloudflare's developer platform to create a distributed, asynchronous, and versioned intelligence system.

## 2. Core Principles
*   **GitHub is the UI:** The human operator interacts exclusively through GitHub (Issues, PRs, Projects). 
*   **Email is the Bus:** Agents use Cloudflare Email Service for robust, asynchronous, and authenticated handoffs.
*   **Artifacts are the Memory:** Shared knowledge is stored in Cloudflare Artifacts (Git-compatible versioned storage).
*   **Workers are the Alchemists:** Logic and transformation happen in Cloudflare Workers, orchestrated by the Agents SDK.

## 3. The Stack

### A. The Interface: GitHub Control Plane
*   **Issues = Intent:** All human requests start as GitHub Issues. They are the "source of truth" for what needs to happen.
*   **PRs = Deliverables:** Agents do not "chat" results. They submit Pull Requests. This ensures all output is versioned, reviewed, and auditable.
*   **Projects = Context:** GitHub Projects provide the high-level roadmap and state tracking for the Factory.

### B. The Transport: Cloudflare Email Service
*   **Agent Identity:** Each agent has a unique email identity (e.g., `sivart@factory.net`).
*   **Asynchronous Handoffs:** When an agent needs to delegate or wait for external input, it sends an email. This provides guaranteed delivery and decouples agents from each other's uptime.
*   **Secure Routing:** Uses HMAC-SHA256 signed headers to ensure replies route back to the correct agent instance.

### C. The Memory: Cloudflare Artifacts
*   **The Substrate:** A Git-compatible filesystem that stores the Factory's shared knowledge.
*   **Forkable State:** Agents can "fork" the Substrate to experiment with ideas or research without polluting the main branch.
*   **Versioned History:** Every change to our shared memory is a commit, allowing for "time travel" and audit trails.

### D. The Engine: AI Gateway + Workers AI
*   **Unified Inference:** All model calls go through **AI Gateway** for centralized logging, cost tracking, and failover.
*   **Edge Intelligence:** Simple tasks (linting, classification) run on **Workers AI** at the edge for low latency.
*   **Model Agnosticism:** The system can swap between OpenAI, Anthropic, or self-hosted models (via Cog containers) without changing the core logic.

## 4. Automation & Workflows

### The "Ingest" Pipeline
1.  **Trigger:** Human opens a GitHub Issue.
2.  **Worker:** A Cloudflare Worker catches the webhook and creates a "Task Object" in a Durable Object.
3.  **Routing:** The Worker emails the appropriate agent (e.g., `sivart@factory.net`) with the task details.

### The "Execution" Pipeline
1.  **Agent Action:** The agent receives the email, forks the Substrate in Artifacts, and performs the work.
2.  **Handoff:** If help is needed, the agent emails another agent (e.g., `koda@factory.net`).
3.  **Completion:** The agent commits their work to a branch in the Substrate and opens a GitHub PR.

### The "Review" Pipeline
1.  **Automated Linting:** A Worker runs `scripts/lint.sh` and `scripts/pre-commit-check.sh` on the PR.
2.  **Human Review:** The Operator reviews the PR in GitHub.
3.  **Merge & Sync:** Upon merge, a Worker updates the main Substrate and sends a Telegram summary to the Operator.

## 5. Security & Observability
*   **Zero-Secrets:** Agents use Workers Bindings for email and AI; no API keys are exposed in code.
*   **Unified Billing:** All AI spend is tracked in one place via AI Gateway.
*   **Audit Trails:** Every agent action is logged in the Substrate and visible via GitHub history.

## 6. Migration Plan
1.  **Phase 1:** Move current `sivart` repo logic into Cloudflare Workers.
2.  **Phase 2:** Implement Email Service for agent-to-agent handoffs.
3.  **Phase 3:** Migrate shared memory to Artifacts for versioned state.
4.  **Phase 4:** Automate GitHub integration via Workers and Webhooks.

---

_This spec is a living document. It evolves as we learn what it means to be a Cloudflare-native Factory._