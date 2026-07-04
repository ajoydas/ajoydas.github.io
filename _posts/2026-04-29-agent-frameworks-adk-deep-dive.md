---
title: "Evaluating Agent Frameworks in 2026: What Changed at Google Cloud Next 🤖"
tags: [agentic AI, ADK, MCP, google cloud, AI engineering]
style: border 
color: primary
description: 
---
<br/>
<img src="/assets/img/posts/gcp-next26-gemini-enterprise-platform.png" alt="Gemini Enterprise Agent Platform at Google Cloud Next 2026" style="width: 100%; height: auto;"/>
<br/>

Spent a chunk of **Google Cloud Next 2026** building agents with Google's **Agent Development Kit (ADK)**. If you're evaluating agent frameworks this year, here are a few patterns I'm bringing back. 👇

#### Skills as first-class artifacts

Instead of stuffing everything into the system prompt, ADK skills are a YAML metadata block + a Markdown body + (optionally) Python scripts the agent can execute. The metadata is loaded eagerly so the agent knows *when* to load the rest of the skill. It's basically retrieval-over-your-own-runbooks — clean, composable, and exporting to/from existing process docs is genuinely a few minutes of work.

#### MCP everywhere

Every Google Cloud service is now **MCP-enabled** by default, with first-party MCP servers for Maps, Cloud APIs, and more. The Developer Keynote demo wired up an agent to Google Maps + a custom GIS skill on stage in under 10 minutes. The cross-vendor MCP story (Anthropic, OpenAI, Google) is finally converging — bet on it.

#### Governance is part of the framework, not bolted on

**Agent Identity** (cryptographic per-agent IAM principals), **Agent Registry** (single source of truth for tools/skills), and **Agent Gateway** ("air traffic control" for inter-agent traffic, with **Model Armor** inline). If you've been rolling your own per-tool auth scaffolding, you can probably retire it.

The takeaway for builders: in 2026 the model is no longer the differentiator. The **harness, the skills library, and the governance plane** are. 💡

Anyone else playing with ADK or comparable frameworks (LangGraph, OpenAI Agents SDK, Anthropic Claude Managed Agents)? Curious which trade-offs are showing up in your production deployments.
<br/>
