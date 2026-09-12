# Relay V1 — Universal Capability Plane

**Working name:** Relay  
**Category:** AI Agent Infrastructure / Universal Capability Plane  
**Status:** Operator-facing Mission Spec source (Mission Control intake)  
**Product repo:** https://github.com/jaydubya818/factory-pilot  
**Spec location:** `docs/RELAY-V1-SPEC.md`  
**Built by:** Mission Control Software Factory (builder only — not a runtime dependency)  
**Date:** 2026-09-12

---

## 1. Thesis

**Agents are replaceable. Capabilities are durable.**

Give any AI agent superpowers. Connect your digital world once. Agents can change; capabilities don't.

## 2. What Relay is / is not

### Relay IS

A persistent capability, identity, state, execution, event, and integration layer that sits **beneath** agents:

- Account-owned **capability plane**
- Agent identities with **scoped grants** and **short-lived leases**
- Durable memory, knowledge, connectors, files, computers, browsers, sandboxes, messaging, calendar, wallet
- One MCP gateway with dynamic tool projection (plus REST, webhooks, event stream)
- Activity ledger, execution receipts, approvals, budgets
- Cross-agent continuity via handoff packages

### Relay is NOT

- An LLM or chatbot product
- A coding agent
- A software factory, FDLC, or Mission Control component
- An orchestration / multi-agent framework that owns agent brains
- Dependent on https://fdlc.ai/ or https://fdlc.ai/mission-control at runtime

Mission Control builds Relay. Relay source must **not** import Mission Control packages or couple to FDLC/MC at runtime.

## 3. Product promise

Connect capabilities once under an account. Authorize agents with grants and leases. Swap or upgrade agents without rewiring the world. Every sensitive action leaves a receipt.

## 4. Onboarding (V1)

1. Create account  
2. Create agent identity  
3. Connect capabilities  
4. Connect runtime  
5. Agent discovers authorized capabilities  
6. Start working  

## 5. Protocol stance

- **MCP** is the V1 agent protocol surface, **not** the internal domain model.
- Also expose: REST, webhooks, event stream.
- Internal domain uses stable IDs and `domain.resource.action` capability names.
- Protocol adapters project domain capabilities outward; they do not redefine the plane.

## 6. Authority model

- **Account owns the capability plane.**
- Agents receive **scoped grants** + **short-lived leases**.
- No universal unrestricted token.
- No raw secrets handed to agents; credentials stay in the plane / connector vault.
- Approvals gate high-risk actions; budgets constrain spend and rate.

## 7. Superpowers (capability families)

Memory · Knowledge · Email · Calendar & Contacts · Messaging · Browser · Computer · Sandboxes · Files · Connectors · Wallet

Not every superpower ships in the first production milestone. The **domain model MUST support all of them** from day one.

## 8. Domain objects (stable IDs)

User, Organization, Agent, Agent Runtime, Agent Session, Capability, Capability Grant, Capability Lease, Capability Profile, Connector, Connection, Credential, Memory, Knowledge Source, Computer, Browser, Sandbox, File, Channel, Event, Event Subscription, Trigger, Workflow, Approval, Skill, Activity, Execution Receipt, Budget.

Capability naming convention: `domain.resource.action`.

## 9. V1 dashboard IA (desktop-first)

```
RELAY
  PLANE        → Overview, Activity, Agents, Skills
  CAPABILITIES → Memory, Knowledge, Email, Calendar, Messages,
                 Browser, Computer, Sandboxes, Files, Connectors, Wallet
  AUTOMATION   → Events, Triggers, Workflows, Approvals
  DEVELOPER    → MCP, API, Webhooks
  CONTROL      → Permissions, Usage, Audit, Settings
```

## 10. Performance (release requirement)

| Surface | Target |
| --- | --- |
| Cached nav (server) | < 100ms |
| Core page | < 200ms p95 |
| API reads | < 250ms p95 |
| Search | < 300ms p95 |
| UI feedback | < 100ms |

Hard rules:

- No N+1 dashboard queries
- Cached shell + parallel fetch
- Provider APIs must **not** block navigation

## 11. Continuity & control features

- Cross-agent continuity
- Connect-once connectors
- Permissions matrix
- One MCP gateway with dynamic tool projection
- Handoff packages between agents/sessions
- Activity ledger
- Execution receipts

## 12. First implementation milestone (factory-pilot)

Scaffold a **Next.js App Router TypeScript** app in `jaydubya818/factory-pilot` with:

1. `/api/health`
2. Dashboard shell implementing the IA above (routes + navigation; stub content OK)
3. Domain model types/modules for **all** objects listed in §8
4. MCP gateway **stub** (health + dynamic tool projection placeholder)
5. No runtime coupling to Mission Control / FDLC packages

Stop condition for Mission V1: domain models + shell + health + MCP gateway stub are verified (TEST / COMMAND / BROWSER).

## 13. Non-goals (V1 milestone)

- Shipping every superpower provider integration end-to-end
- Building an LLM chat product inside Relay
- Embedding Mission Control or FDLC as a dependency
- Universal unrestricted agent tokens
- Mobile-first dashboard (desktop-first V1)

## 14. Constraints

- Public product repo: `jaydubya818/factory-pilot` (branch `main`)
- TypeScript + Next.js App Router
- Secrets never returned raw to agents
- Spec intake and factory planning grant **no** execution authority until Plan approval
- Performance SLOs are acceptance-grade, not aspirational

## 15. Risks

| Risk | Severity | Mitigation |
| --- | --- | --- |
| Domain model under-specified for deferred superpowers | HIGH | Model all objects in V1 even if providers are stubs |
| Provider latency blocks UI | HIGH | Cached shell, parallel fetch, never await provider on nav |
| Secret leakage via MCP tools | CRITICAL | Vault + leases; project only authorized tool schemas |
| Spec paste cutoff (multi-agent / Computer exclusive) | MEDIUM | Tracked as OPEN clarification CLARIFY-CUTOFF-063 |

## 16. OPEN clarification (source paste cutoff)

**CLARIFY-CUTOFF-063 — OPEN**

Original operator paste was cut off at section 63 covering **multi-agent concurrency / Computer exclusive** semantics.

Until resolved, do not implement exclusive Computer lease arbitration beyond a stub domain model and documented assumption placeholders. Product must answer:

- Exclusive vs shared Computer sessions across agents
- Queueing, preemption, and handoff rules
- Lease duration and revocation under concurrency

## 17. Sources

- This document: `docs/RELAY-V1-SPEC.md` on `jaydubya818/factory-pilot`
- Product context only (not dependencies): https://fdlc.ai/ · https://fdlc.ai/mission-control

## 18. Definition of done (V1 Mission Spec → Plan gate)

- Mission Spec finalized in Mission Control for project **Relay** (`relay`)
- factory-pilot contains this spec on `main`
- Next factory step: propose / approve a **Plan** (no WorkOrder dispatch in this intake)
