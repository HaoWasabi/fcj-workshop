---
title: "Event 4"
date: 2026-08-08
weight: 4
chapter: false
pre: " <b> 4.4. </b> "
---

# Harvest Report: "Agent Forge - Deepdive (Day 2)"

### Purpose of the Event

AWS FCAJ Agent Forge – Deepdive (Day 2) is the second session in the in-depth workshop series organized by the First Cloud AI Journey (FCAJ) community. The program is at the advanced level (L300 – Advanced), built on materials and real-world experience from AWS engineers.

The workshop was organized to help participants:
- Gain additional knowledge on memory management, response quality evaluation, system monitoring, and performance optimization.
- Practice developing an AI Agent from a basic level into an Agentic AI system capable of operating in an enterprise environment.

### List of Speakers

- **Hieu** – Co-head of the FCAJ community, Solution Architect at AWS Vietnam.
- **Hai Anh** – Cloud Consultant at Chiase Pacific, directly guiding the hands-on lab portion.
- **Nghia Tran** – Agentic AI Solution Architect
- **Anh Pham** – Cloud Consultant G-AsiaPacific Vietnam

### Workshop Format

This is a **3-day workshop series**, designed with a roadmap ranging from fundamental knowledge to deploying AI Agents in production environments using Amazon Bedrock AgentCore.

- **Day 1 (08/01): AgentCore Foundations**  
  Explore the overall architecture of Amazon Bedrock AgentCore, including **Runtime**, **Gateway**, and **Identity**, along with foundational concepts for building AI Agents.

- **Day 2 (08/08): Memory, Evaluations, Observability & Optimization**  
  Discover how to manage **Memory**, evaluate AI Agent performance (**Evaluations**), monitor systems (**Observability**), and optimize performance (**Optimization**).

- **Day 3 (08/15): DevOps, Policies & Production Best Practices**  
  Learn the **DevOps** workflow for AI Agents, build **Policies**, apply security measures, and follow **best practices** for deploying AI Agents in production environments.

## Key Content Highlights

### 1. Overview of Agentic AI

#### Agent Memory

Agent Memory helps Agents overcome the limitations of the Context Window, maintain conversation context, and personalize user experiences.

**Short-term Memory** stores the entire conversation history synchronously as raw messages. This allows the Agent to understand the current exchange and respond consistently. The system also supports a branching mechanism, similar to how Git creates branches during software development.

**Long-term Memory** operates asynchronously. The system extracts important information from conversations and stores it as vectors for retrieval in future sessions. The four main storage strategies are:

- **Summary:** summarizes and compresses conversation content.
- **User Preference:** stores user preferences.
- **Semantic:** stores domain-specific knowledge.
- **Episodic:** records past decisions or events.

**Namespace** is used as a hierarchical directory structure to isolate data by strategy, actor, or session. When combined with semantic search and similarity ranking, the Agent can find the exact information it needs, reducing token usage and improving response time.

#### System Observability

The workshop emphasizes the principle: *"You cannot fix what you cannot see."* The Observability system uses the OpenTelemetry standard to collect three main data groups:

- **Logs:** records details about requests, connection errors, system errors, or terminal logs.
- **Traces:** tracks the entire journey of a request, from when the user sends a prompt to when the Agent returns a response, including tool calls.
- **Metrics:** measures indicators such as token consumption, error rates, and response latency.

This data helps the development team identify causes of delays, optimize token costs, and improve the user experience.

#### Agent Evaluation System

A common risk with AI Agents is **hallucination** — providing inaccurate information as if it were fact. To mitigate this risk, the system provides 13 built-in evaluators, such as **correctness** and **helpfulness**.

Evaluators are applied at three levels:

- **Session level:** evaluates the results of the entire working session.
- **Trace level:** evaluates the accuracy of individual responses.
- **Span level:** evaluates each processing step, such as tool calls or parameter passing.

The system supports two evaluation modes. **On-demand** is suitable for development and testing phases; **Online** is used to monitor Agent quality in real time in production environments. Automated evaluation results still require domain expert verification to ensure accuracy.

## What Was Learned

### Professional Knowledge

- Clearly understanding the difference between Short-term Memory and Long-term Memory, especially the synchronous and asynchronous processing mechanisms.
- Grasping the three pillars of Observability — Logs, Traces, and Metrics — along with the role of the OpenTelemetry standard in monitoring system health.
- Understanding how evaluators automatically assess Agent responses against standardized criteria rather than relying entirely on subjective impressions.
- Learning about Cedar Policy and the sandbox mechanism, thereby recognizing the role of security when Agents execute tasks or test source code.

### Key Takeaways

- Design AI Agents around smaller functional modules before building complex systems.
- Always prioritize security and access control when AI Agents access resources.
- Monitor, evaluate, and optimize AI Agents based on real-world outcomes.
- Build AI Agents with scalability and maintainability in mind.

## Workshop Experience

Participating in **Day 2 of the AWS FCAJ Agent Forge – Deep Dive** gave me a comprehensive view of how to build and operate AI Agents in enterprise environments.

Through the speakers' presentations and hands-on content, I gained a clearer understanding of how to create an effective AI Agent by equipping the system with a knowledge storage mechanism, monitoring, quality evaluation, and tight security controls.

### Event Photos
![Event Photo 1](/images/4-EventParticipated/image004.jpg)

> **Overall Assessment:** Day 2 of **AWS FCAJ Agent Forge – Deep Dive** provided a solid foundation in **Agentic AI** and **Amazon Bedrock AgentCore**, helping participants understand from fundamental concepts to architecture and how to deploy AI Agents in production environments. The workshop combined theory, illustrative examples, and hands-on content, while emphasizing important factors such as security, scalability, lifecycle management, and tool integration. This is a valuable program for anyone looking to build AI Agent systems that meet enterprise environment requirements.
