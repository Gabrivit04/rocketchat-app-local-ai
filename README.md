# Rocket.Chat Local AI App (GSoC 2026 Proposal)

A Rocket.Chat App that connects self-hosted workspaces to **locally hosted LLMs** (Ollama, LM Studio, and compatible OpenAI-style local endpoints), enabling AI features without sending data outside the organization.

---

## Community Pitch (polished)

Hi everyone! 👋

I’m very excited to join this community and to prepare my **GSoC 2026** proposal around Rocket.Chat.

After studying Rocket.Chat’s architecture and its strong commitment to privacy, self-hosting, and enterprise readiness, I’d love to propose an idea and ask for feedback from mentors and maintainers.

### The problem
Many teams want AI capabilities in Rocket.Chat (thread summaries, translation, Omnichannel support suggestions, etc.).

Today, the most common approach is integrating cloud AI APIs. For privacy-sensitive and regulated deployments, this can be a blocker because message content leaves the organization boundary.

### Proposal: `Rocket.Local-AI`
Build a dedicated Rocket.Chat App (TypeScript, Apps Engine) that acts as a bridge between Rocket.Chat and **on-prem/local LLM runtimes**.

By integrating with providers like **Ollama** and **LM Studio**, workspaces could enable practical AI features while preserving a strict privacy model: **no external data transfer**.

### MVP scope
- **Plug-and-play local endpoint setup** (admin settings, health checks, model selection)
- **On-demand thread summarization**
- **Omnichannel agent reply drafting** from conversation history

### Why this aligns with Rocket.Chat
- strengthens Rocket.Chat’s self-hosted value proposition
- opens AI adoption to enterprises with strict compliance requirements
- keeps the integration modular through the Apps Engine

I’m currently deep-diving into Apps Engine internals and looking for good first issues to contribute while shaping this proposal.

I’d be grateful for feedback on:
- roadmap alignment,
- technical constraints,
- and mentor interest in this direction.

Thank you!

---

## Expanded implementation concept

### 1) Architecture (high-level)
- **Rocket.Chat App layer**
  - command handlers (`/ai summarize`, `/ai draft`)
  - contextual actions (message/thread actions)
  - settings UI (endpoint URL, model name, timeout, max tokens)
- **Provider abstraction**
  - `LocalAIProvider` interface
  - adapters: `OllamaProvider`, `LMStudioProvider`
  - optional OpenAI-compatible local adapter for extensibility
- **Prompt pipeline**
  - context collection (thread/messages/tags)
  - token budgeting + truncation strategy
  - deterministic templates for summary and draft tasks
- **Safety & controls**
  - role-based feature access
  - per-room/per-department enablement
  - request/audit logs (without storing sensitive prompt payloads by default)

### 2) MVP functional requirements
- Admin can configure one or more local LLM endpoints.
- App validates endpoint and selected model availability.
- Users can request a thread summary in-channel.
- Omnichannel agents can generate one-click draft replies.
- Clear user-facing fallback messages on timeout/model offline.

### 3) Non-functional requirements
- Privacy-first defaults (no outbound telemetry of message content).
- Graceful degradation when model is unavailable.
- Configurable latency/timeouts for production usage.
- Extensible provider layer to support future local runners.

### 4) Suggested milestone plan (GSoC-friendly)
1. **Community bonding & discovery**
   - validate use-cases with mentors and users
   - finalize API boundaries and app settings schema
2. **Core integration**
   - provider interface + Ollama adapter
   - endpoint health checks + model listing
3. **MVP features**
   - thread summary action
   - Omnichannel draft generation
4. **Hardening**
   - retries, timeout handling, RBAC checks
   - docs and example deployment recipes
5. **Stretch goals**
   - multilingual translation command
   - caching + prompt profiles per department/team

### 5) Success criteria
- At least one production-like self-hosted setup can run the app end-to-end with local LLM only.
- Users can summarize threads and draft replies with acceptable latency.
- Documentation is sufficient for maintainers and self-hosted admins.

---

## Future extensions (post-MVP)
- RAG over internal knowledge bases (strictly on-prem)
- model routing (fast vs. high-quality local models)
- policy-driven prompt filters and compliance modes
- evaluation suite for response quality and latency benchmarking

