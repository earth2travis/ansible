---
title: "Asynchronous Agent Communication"
tags: [agents, communication, email, workflow]
related: [[cloudflare-email-service-for-agents]], [[micrograd-principle]], [[cloudflare-developer-platform]]
source: research/raw/cloudflare-email-service-for-agents.md
---

# Asynchronous Agent Communication

The shift from **synchronous chat** to **asynchronous email** is the maturity model for AI agents.

## The Core Shift
- **Synchronous (Chatbots):** The agent is a "reactor." It waits for a prompt and must respond immediately. This limits it to simple Q&A and forces the human to stay engaged in the session.
- **Asynchronous (Agents):** The agent is an "actor." It receives a signal, performs work (which may take seconds or days), and then communicates the result. This allows for **long-running workflows** and **proactive intelligence**.

## Why Email?
Email is the "TCP/IP" of human communication. It is universal, persistent, and identity-based. By building agents that are "email-native," we allow them to:
1.  **Integrate with Human Time:** Humans don't work in 24/7 chat streams; they work in inbox bursts.
2.  **Maintain Context:** The email thread provides a natural, chronological history of the agent's work and decisions.
3.  **Scale Securely:** Using address-based routing and signed headers, we can manage millions of agent interactions without the security risks of open-ended chat interfaces.

## The Synthweave Application
Synthweave should view the inbox not as a "notification channel," but as the **primary control plane** for the organization. The "World Model" speaks to the humans through the Substrate, but it *collaborates* with them through the inbox.