---
title: "Google Cloud Next 2026: The Full Download — Agents, Governance, Data & Infra 📝"
tags: [google cloud, agentic AI, security, data engineering, AI infrastructure]
style: border 
color: secondary
description: 
---
<br/>
<img src="/assets/img/posts/gcp-next26-keynote.png" alt="Google Cloud Next 2026 keynote" style="width: 100%; height: auto;"/>
<br/>

I've already shared my [top 3 takeaways](/blog/google-cloud-next-2026-recap) and a [wider summary](/blog/google-cloud-next-2026-learnings) from **Google Cloud Next 2026** — this one is the full download: my detailed notes across every theme I followed at the conference. Grab a coffee. ☕
<br/>

#### TL;DR — Five Headline Shifts 👇

1. **Vertex AI is now the "Gemini Enterprise Agent Platform"** — same lineage, reorganized around four pillars: **Build · Scale · Govern · Optimize**. Every Google Cloud service is **MCP-enabled by default**. ADK is the canonical framework, with Agent Garden (pre-built agents) and Agent Studio (low-code).
2. **Agentic governance moved from optional to default.** New primitives — **Agent Identity** (cryptographic ID per agent, IAM-native), **Agent Registry** (single source of truth for agents/tools/skills), **Agent Gateway** ("air traffic control" for inter-agent traffic, with Model Armor inline) — close most of the OWASP Agentic Top 10 attack surface in one platform.
3. **"Data" is being redefined as a system of action.** BigQuery + Gemini, the Knowledge Flywheel (semantics + relationships + usage), and unstructured-data activation (the "90% dark data") are pushed as the substrate for agents. **American Express** is migrating its core warehouse to BigQuery; **Mercari** handles 38% of customer inquiries with autonomous agents; **Virgin Voyages** runs **1000+ specialized agents** in production.
4. **Long-running agents are a 2026 reality.** **Anthropic**'s Claude can now run autonomously for **4+ hours** (Opus 4.6). The hard part is no longer the model — it's the **harness** (artifacts, planners, evaluators, sandboxes). Anthropic's new **Claude Managed Agents** API on the Gemini Enterprise Agent Platform productizes this pattern.
5. **Compute moved a generation.** TPU v8 (Ironwood) + GKE Dynamic Slicing significantly improve price/perf and recovery time for large-scale training and inference. Net effect for the rest of us: **LLM training and inference get faster and cheaper, so the underlying economics of building AI agents improve**.
<br/>

#### Executive Overview

Google's pitch this year was that the enterprise has crossed from **system of intelligence** (reactive: ask a question, get an answer) to **system of action** (proactive: agents that complete multi-step work autonomously). Three currents underpin everything:

- **Agent scale, not human scale** — workloads measured in agent-hours, not user-sessions.
- **From data to knowledge** — semantic relationships, business meaning, and usage patterns become first-class metadata.
- **Open and portable by default** — MCP everywhere, A2A protocol shipped, partner models (Claude, Mistral, Llama, DeepSeek, Qwen, Grok) all hosted on the same control plane.

For an engineering team building AI features today, the practical implication is that the *frameworks* most teams picked in 2024–2025 (LangGraph, raw function-calling SDKs, etc.) are now table stakes. The differentiator in 2026 is **governance + harness design + observability** — and Google now ships those out of the box.
<br/>

#### 🤖 Agentic AI Platform — the New "Vertex"

- **Gemini Enterprise Agent Platform** = renamed/expanded Vertex AI; explicit four pillars: Build · Scale · Govern · Optimize.
- **Models in the platform**: Gemini 3.1 Pro, Gemini 3.1 Flash, **Gemini 3.1 Flash Image (Nano Banana 2)**, **Veo 3.1** (video), **Lyria 3** (music). Partner models (Claude Opus/Sonnet/Haiku, Mistral, Grok), open models (Llama, DeepSeek, Qwen) — all behind the same control plane.
- **Agent Development Kit (ADK)**: model-agnostic, modular framework. **Skills** (YAML metadata + Markdown body + scripts) are first-class artifacts. The keynote demo built a multi-agent marathon planner end-to-end with ADK + a Maps MCP server + a custom GIS skill in under 10 minutes on stage.
- **Agent runtime**: serverless, with built-in **sessions** (user continuity) and **memory** (personalization).
- **Agent Garden**: catalog of pre-built agents. **Agent Studio**: low-code canvas for designing multi-agent reasoning loops. **RAG Engine** + **Vector Search** for grounding.
- **MCP everywhere**: every Google Cloud service is MCP-enabled by default; first-party MCP servers shipped for Maps, Cloud, etc.
- **A2A protocol**: standardized agent-to-agent discovery + collaboration via the Agent Registry.
<br/>

#### 🛡️ Governance & Security — the OWASP Agentic Top 10

**What it is.** The **OWASP Agentic Security Initiative (ASI) Top 10** is the agent-era successor to the familiar OWASP LLM Top 10 and Web Top 10. It catalogues the ten most critical risks specific to **autonomous, tool-using AI agents** — the new attack surface that emerges when an LLM is given tools, memory, identity, and the ability to call other agents. Each risk gets an **ASI** ID (ASI01–ASI10).

In the Next '26 session, Google walked through each risk and mapped it to a concrete control on the Gemini Enterprise Agent Platform. Here's my shareable summary:

| **ID** | **Risk — what it means** | **How to mitigate** |
| --- | --- | --- |
| **ASI01 — Agent Goal Hijacking** | An attacker overrides the agent's intended objective via prompt injection (direct in user input, or indirect via documents, tool outputs, web pages, emails the agent reads). | Inline prompt-injection filtering at every input boundary (e.g. **Model Armor** with prompt-injection, jailbreak, and unsafe-URL classifiers); separate trusted instructions from untrusted content; constrain the agent's goals declaratively rather than in free-form prompts. |
| **ASI02 — Tool Misuse / Exploitation** | The agent uses a legitimate tool in an unintended or harmful way (deletes records it should only read, exfiltrates data via a "send email" tool, calls APIs at abusive rates). | Per-tool **least-privilege** scopes; declarative **semantic policies** in natural language ("Do not share customer PII with third-party tools") enforced at the gateway; **behavioral anomaly detection** on agent traces; rate limits and budget caps per tool. |
| **ASI03 — Identity & Privilege Abuse** | Agents act with overly broad credentials, or impersonate users/other agents. Without an identity primitive, the agent inherits whatever service-account it was started with — usually too much. | Give every agent a **first-class identity** (IAM principal), task-scoped and time-bound. Require explicit per-action authorization. Audit every action against that identity. (Google's primitive: **Agent Identity**.) |
| **ASI04 — Agentic Supply Chain** | Compromise of any upstream artifact the agent depends on: a poisoned model, a malicious MCP server, a tampered skill or system prompt, a backdoored framework dependency. | **Signed artifacts and provenance** for models, skills, prompts, and tools; a curated **Agent Registry** as the only source of truth; watermarking; supply-chain scanning (SLSA-style attestations) on the framework itself. |
| **ASI05 — Unexpected Code Execution** | The agent runs code (its own or attacker-supplied) outside the boundaries the operator intended — e.g. an `exec()` tool, a code-interpreter, or an MCP tool that shells out. | Run all agent-generated code in a **kernel-isolated, air-gapped sandbox** that is wiped after each run; default-deny outbound network; VPC service controls; security-command-center monitoring for AI workloads. |
| **ASI06 — Insecure Inter-Agent Communication** | In multi-agent systems, agent-to-agent (A2A) traffic is often unauthenticated, unencrypted, and untraced. A compromised "helper" agent can poison its caller. | Route all A2A and Client→Agent traffic through an **Agent Gateway** that enforces mutual auth, policy, and content filtering; standardize on a typed protocol (A2A) rather than free-form prompts between agents; immutable trace logs. |
| **ASI07 — Memory & Context Poisoning** | An attacker writes hostile content into the agent's long-term memory, RAG store, or shared session memory. The next time the agent reads it, the poisoned content acts as a delayed prompt injection. | **Per-session/per-tenant memory isolation** with encryption and IAM RBAC; sanitize and Model-Armor inputs **at write time**, not just read time; expiry/TTL on memories; signed memory entries. |
| **ASI08 — Cascading Failures** | One bad output (a hallucination, a wrong tool call, a misclassification) propagates through a chain of agents and amplifies. Common in planner→builder→executor pipelines. | Multi-dimensional **evaluation** at each hop (final-response quality, tool-use correctness, trajectory quality, task success, hallucination rate); circuit-breakers and HITL gates on high-impact actions; bounded retry budgets. |
| **ASI09 — Human–Agent Trust Exploitation** | Humans over-trust agent output: confidently-wrong text, manipulated UX, or agent-generated content presented as human-authored, leading to social-engineering, fraud, or bad business decisions. | Clear **agent identification** in UX ("this was generated by an agent"); **content watermarking**; HITL gates on irreversible or high-stakes actions; calibrated confidence display; user education. |
| **ASI10 — Rogue Agents** | An agent goes off-script — either compromised, misconfigured, or simply hallucinating — and starts taking actions outside policy: exfiltration, lateral movement, abusive API usage. | Continuous **behavioral anomaly detection** with explainable signals; immutable audit logs; kill-switch + auto-quarantine via the gateway; registry-based allow-list of agents permitted to run in production. |

**The cross-cutting lesson.** Most ASI mitigations are *infrastructure*, not prompt engineering — identity, gateway, sandbox, registry, anomaly detection, eval harness. Treating agentic security as "a better system prompt" is the dominant anti-pattern. ⚠️
<br/>

#### 🔧 SRE & Operations — the Agentic SRE Swarm

**Pattern**: instead of a monolithic AIOps tool, deploy a **swarm of specialized agents** behind one **SRE Orchestrator**. Roles demonstrated in the joint **Google + PayPal** session:

- **Architect & Design Agent** — automated production-readiness review; scans code/configs for reliability anti-patterns before commit; can prompt the coding agent to auto-generate "reliable" config.
- **Launch & Rollouts Agent** — detects non-deterministic failures during canary/blue-green at **~1% rollout** (vs ~10% for humans); auto-rollback.
- **Experiment & Feature-flag Agent** — bounds blast radius via 1% exposure.
- **Resource Allocation Agent** — distinguishes organic spikes (e.g. a viral product drop) from inorganic (DoS); coordinates capacity / DDoS defense.
- **Observability Agent** — scans services for missing OTel; auto-injects sidecars; learns SLIs from real deployment behavior.
- **Maintenance Agent** — topology-aware drain/undrain via the Application Topology Graph.
- **Health Agent** — unified health score across infra, DC, cloud, and app layers.
- **Troubleshooting & Incident Agent** — parallel-hypothesis exploration ("network?" vs "code?" simultaneously); turns theories into `gcloud`/Terraform fixes; drafts customer comms; auto-postmortem.

**Levels of autonomy** were called out explicitly: HITL → Recommend-and-ask → Auto-apply-and-report → Fully autonomous. The pitch is that a good harness lets you ratchet up over time as confidence grows.

**Reference scale (PayPal)**: 450M users · $5M revenue/min · 3000 microservices · 2B daily API interactions · 10.5M changes/year. "Cyber Five" (BFCM) is 3–4× normal traffic.
<br/>

#### ⏱️ Long-Running Agents — Harness Design (Anthropic)

The most actionable session of the week. Frontier models now run autonomously for 4+ hours (Claude Opus 4.5/4.6).

**Five failure modes under pressure**:

1. **Context rot** — recall degrades as the window fills (transformers' quadratic attention).
2. **Premature completion** — "context anxiety": the model wraps up early sensing the limit.
3. **Lossy compaction** — summarization drops details the next session needs.
4. **Shallow plans** — one-shot attempts instead of decomposition.
5. **Lossy self-evaluation** — models grade their own work too kindly; bugs ship as "done".

**Harness principles** (the framework worth internalizing):

- **Use structured artifacts, not the context window.** Each session ends by writing `progress.json` + `features.json` + a git checkpoint. The next session reads state, picks the next feature, starts with a fresh window and full awareness.
- **Decompose scope before any code is written.** A dedicated **Planner** agent turns the brief into 200+ testable feature specs. Builders pick one feature per session.
- **Separate generation from evaluation.** A fresh-context **Evaluator** with a rubric and real tools (Playwright, etc.) — uncorrelated, free to disagree with the Builder.
- **Do the simplest thing first.** Every model has a zone of reliable execution; the harness extends it outward. When the model upgrades, **revisit the harness and remove what you can** (Sonnet 4.5 needed context resets; Opus 4.6 didn't).

**Claude Managed Agents** (new product on the Agent Platform): declare an agent profile (model + system prompt + tools + callable agents), then `user.define_outcome` carries the brief + rubric. Build, grade, revise loops happen server-side until the rubric is satisfied. Anthropic handles state, sandbox, and scale.
<br/>

#### 🧠 Data & Analytics — the Agentic Data Cloud

- **System of action** thesis: agents need to *take* action, not just report. Reactive analytics is last-generation.
- **Knowledge flywheel**: data → semantics + relationships + usage patterns → trustworthy grounding for agents.
- **"90% dark data" activation**: contracts, product specs, emails, images, video — Gemini's multimodal long-context unlocks them as queryable assets.
- **BigQuery + Gemini**: 30× YoY growth in BigQuery data processed by Gemini.
- **Customer signals**: **American Express** migrating its core warehouse to BigQuery; **Mercari** at 38% autonomous customer-inquiry handling; **Virgin Voyages** flagship agent "Ruby" cut mass itinerary rebooks from 6 hr → 11 min, with 1000+ specialized agents in production.
<br/>

#### 🔒 Sensitive Data Protection (DLP)

- Now positioned as the **substrate underneath Model Armor** for sensitive-data classification.
- **200+ built-in classifiers**, AI-powered context (finance / health / legal / source code / medical record / etc.).
- **AI context detectors** distinguish, e.g., "my arm is broken" (health) from "my record-player arm is broken" (customer service) using surrounding context, not keywords.
- **Image inspection**: object + theme detection (faces, ID cards, passports, barcodes); pixel-level redaction.
- **Audio inspection** (preview): speech-to-text → text classifier.
- **Integrations**: REST API, Vertex AI, Apigee, GKE gateways, Cloud Run inline; Conversational AI (CCAI/Dialogflow); Gemini Enterprise (preview).
<br/>

#### 🪙 Web3 & Agents — Payments, Tokenization, Agentic Companies

Mostly contextual / forward-looking. Top ideas worth tracking:

- **Agents transacting with real money** — the open problem is *unhappy paths*: who covers the loss, how disputes are filed, how to verify an agent acted within delegated authority. Several startups (EigenLayer, Aptos/Shelby, t54 Labs, ant-finance) are pitching insurance + verifiable execution layers.
- **"Agentic companies"** (Sreeram Kannan, EigenLayer): the idea of collapsing the company stack (capital + governance + execution + property rights) into software. Speculative, but worth a mental bookmark.
- **Tokenized data marketplaces**: privacy-preserving fine-tuning on tokenized datasets (medical, financial). Plausibly relevant to industry data partnerships in the 3–5 year horizon.
<br/>

#### 🌍 Open Ecosystem — LangChain, Hugging Face, Chainguard

- **Open vs closed models is a gradient, not a binary.** Start on a frontier model API for prototyping; move to open weights when (a) cost matters, (b) you need control over change-management, or (c) you need to fine-tune.
- **Stack consensus** for 2026 agent apps: model + vector DB + tools/APIs + agent framework + observability + evals + security + agent identity.
- **Least-tool-call principle** (Matt Moore, **Chainguard**): giving an agent a shell = giving it any tool. Be intentional about the smallest viable toolkit per agent.
- **Eval is non-negotiable**: prompts can't be reasoned about like code; treat evals as the regression suite for model/prompt/tool changes.
<br/>

#### ⚡ AI Infrastructure — TPU v8 (Ironwood) & GKE Dynamic Slicing

Briefly, because most teams don't operate this layer themselves. The headline is generational:

- **TPU v8 (Ironwood)** ships in two SKUs — **Zebrafish** (training) and **Sunfish** (inference + RL) — with ~4× datacenter networking, ~2.7× price/perf, and ~2× perf/watt vs the previous-gen v5p. Topology scales from a 64-chip "cube" up to a 9216-chip v7x configuration.
- **GKE Dynamic Slicing** decouples node-pool topology from workload topology. Job startup goes from 10–30 min → **20–60 sec**, and failure recovery from 20+ min → **2–5 min**. **GKE Hypercluster** scales to 250K nodes / 1M chips per cluster. **GKE Inference Gateway** reports 30% lower serving cost, 60% lower tail latency, 40% higher throughput vs vanilla open-source Kubernetes.

**Net effect for application teams**: LLM **training and inference get meaningfully faster and cheaper** in 2026. The economics of building, running, and scaling AI agents improve accordingly — the same agent that was marginal at 2025 prices is comfortable at 2026 prices, and previously-impractical workloads (long-running agents, large agent swarms, real-time multimodal) become viable.
<br/>

That's the full download! 🎉 If you made it this far, I'd love to hear which of these themes matters most for what you're building — and if you were at Next and caught a great session I missed, point me to the recording. 📺

Grateful to have had the chance to attend and bring all of this back. 🙏
<br/>
