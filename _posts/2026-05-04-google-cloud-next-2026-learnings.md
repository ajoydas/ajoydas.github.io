---
title: "Beyond the AI Headlines: SRE, Security, Infra & Data Learnings from Cloud Next 2026 🌐"
tags: [google cloud, SRE, security, TPU, data engineering]
style: border 
color: secondary
description: 
---
<br/>
<img src="/assets/img/posts/gcp-next26-agentic-sre.png" alt="Agentic SRE session at Google Cloud Next 2026" style="width: 100%; height: auto;"/>
<br/>

A week after **Google Cloud Next 2026** — here's a wider sweep of what stood out, beyond the agentic AI headlines. 🌐

The agentic AI announcements got most of the airtime, but Next '26 quietly shifted a few other domains too. My broader synthesis:
<br/>

#### 🛡️ SRE → agent swarms

**Google + PayPal** showed a working pattern: instead of one monolithic AIOps tool, deploy a *swarm* of specialized agents (Architect, Rollouts, Observability, Maintenance, Health, Troubleshooting, Incident) coordinated by an SRE Orchestrator. PayPal's numbers — 450M users, $5M revenue/min, 3000 microservices — make a compelling case that linear human SRE can't keep scaling. Detection at ~1% rollout vs ~10% for humans is the metric to anchor on.
<br/>

#### 🔒 Security finally has a vocabulary for agents

**OWASP's Agentic Top 10** (ASI01–ASI10) is now the shared language. Google mapped concrete controls to each risk: **Model Armor** for prompt injection (ASI01), **Agent Identity** for privilege abuse (ASI03), **Agent Sandbox** for unexpected code execution (ASI05), **Agent Memory Bank** for context poisoning (ASI07). Useful even if you're not on Google Cloud — at minimum, audit your agents against the framework.
<br/>

#### ⚡ Infrastructure made a generational jump

**TPU v8 (Ironwood 8)** lands with ~4× data center networking and ~2.7× price/performance vs v5p. **GKE Dynamic Slicing** decouples infra provisioning from workload scheduling — job startup goes from 10–30 minutes to 20–60 seconds; failure recovery from 20+ minutes to 2–5 minutes. **GKE Hypercluster**: 250K nodes, 1M chips per cluster.
<br/>

#### 🧠 Data became a "system of action"

The framing: stop building dashboards as the deliverable; the deliverable is an *action*. **Amex** migrating its core warehouse to BigQuery. **Mercari** at 38% autonomous customer-inquiry handling. **Virgin Voyages** running 1000+ specialized agents, cutting mass itinerary rebooks from 6 hours to 11 minutes.
<br/>

What surprised me most: how fast the *governance* and *infrastructure* layers caught up to the model layer. In 2024 we were arguing about which framework to use. In 2026 the framework is settled — what differentiates teams is harness design, governance, and observability.

Which of these four shifts is most relevant to what you're building? 💭
<br/>
