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

Official delivery follows **WO-001–WO-014** (§210). Early foundation still includes:

1. Repository + technical foundation (Next.js App Router TypeScript, `/api/health`)
2. Domain model + database for **all** objects listed in §8
3. Dashboard shell implementing the IA above
4. MCP gateway with dynamic tool projection
5. No runtime coupling to Mission Control / FDLC packages

Stop condition for first release: golden-path E2E qualification (WO-014) with performance and security qualification evidence (WO-012 / WO-013).

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
| Spec paste cutoff (multi-agent / Computer exclusive) | LOW | RESOLVED via §64 concurrency matrix; Computer exclusive-by-default + supervised handoff |

## 16. Clarification (source paste cutoff)

**CLARIFY-CUTOFF-063 — RESOLVED** with the official §64 concurrency matrix (operator supplied §64–215).

Original operator paste was cut off at section 63. The remainder (§64–215) is now incorporated below. Computer is **exclusive by default** with optional **supervised handoff**. Relay **MUST NOT** assume one agent at a time.

## 17. Sources

- This document: `docs/RELAY-V1-SPEC.md` on `jaydubya818/factory-pilot`
- Product context only (not dependencies): https://fdlc.ai/ · https://fdlc.ai/mission-control

## 18. Definition of done (V1 Mission Spec → Plan gate)

- Mission Spec finalized in Mission Control for project **Relay** (`relay`) through **§215**
- factory-pilot `docs/RELAY-V1-SPEC.md` on `main` includes §64–215
- Official Plan uses WO-001–WO-014 graph (Plan approval is a separate human gate; no dispatch in intake)

---

# Remainder of product spec (§64–215)

Status: **COMPLETE through §215**. Operator-supplied remainder incorporated 2026-09-12. This section is normative for Mission Spec + Plan.

## 64. Concurrency (explicit multi-agent)

Relay **MUST NOT** assume one agent at a time. Resource concurrency matrix:

| Resource | Concurrent semantics |
| --- | --- |
| **Memory** | Concurrent writes allowed with explicit conflict resolution |
| **Computer** | **Exclusive by default**; optional **supervised handoff** to another agent/session |
| **Sandbox** | Multi-agent configurable; file collisions detected and surfaced |
| **Browser** | Single controller; additional agents may be read-only observers |
| **Wallet** | Serialized transactions; idempotent writes required |
| **Files** | Version-aware; conflict detection on concurrent mutation |
| **Email** | Concurrent reads; serialized sends |
| **Connectors** | Bound by provider rate limits **and** Relay policy |

## 65. Resource locks

Every exclusive or contended resource MUST support:

- `acquire` / `release` / `status`
- Auto-expire TTLs so abandoned agents cannot hold the plane hostage
- Auditable lock holders (agent, session, lease)

## 66. Idempotency keys

All consequential writes (wallet, email send, connector mutations, grants, leases, approvals, workflow triggers, file commits) MUST accept an **idempotency key** and return the same durable result on retry.

## 67. Failure model

Failures MUST be machine-readable. Every failed operation returns a structured error with at least: `code`, `category`, `retryable`, `humanMessage`, `correlationId`, and optional `details` (never secrets).

## 68. Retry classes

Retries MUST classify as: `SAFE_IMMEDIATE`, `SAFE_BACKOFF`, `REQUIRES_OPERATOR`, `FATAL`. Retry paths MUST preserve idempotency keys and MUST NOT duplicate side effects.

## 69. Circuit breakers

Provider integrations MUST use circuit breakers so a stalled third party cannot stall the dashboard shell or core plane APIs. Open circuits degrade that subsystem without blocking navigation.

## 70. Subsystem health

Each subsystem exposes health as one of: `HEALTHY` | `DEGRADED` | `UNAVAILABLE` | `UNKNOWN`. Overview and Developer surfaces aggregate these states.

## 71–73. Skills vs capabilities

- **Capabilities** are account-owned durable powers (grants + leases).
- **Skills** are installable instruction/pack artifacts that may *request* capabilities.
- Installing a skill **MUST NOT** auto-grant capabilities. Grants remain explicit operator (or policy) actions.

## 74–77. Agent instructions, state, sessions, snapshots

- Agent **instructions** (prompt/policy text) are versioned and distinct from mutable **state**.
- **Sessions** are ephemeral runtime contexts bound to an agent identity + capability snapshot.
- Capability **snapshots** freeze the grants/leases visible to a session for auditability.
- Handoff packages carry instructions pointer, state summary, and capability snapshot — not raw credentials.

## 78–87. Events, wake, inbox, schedules, triggers, workflows

Relay includes an **event bus** with subscriptions, wake signals, agent inbox, schedules, and triggers. Lightweight workflows are supported for capability-plane automation.

Relay is **not** a BPM / multi-agent orchestration product. Workflows coordinate plane side effects; they do not own agent brains.

## 88–93. Approvals, budgets, wallet policy, audit vs activity

- High-risk actions require **approvals** with durable decisions.
- **Budgets** constrain spend and rate per account/agent/capability.
- **Wallet policy** gates transfers and external payments.
- **Activity** is the operator-facing ledger of what happened.
- **Audit** is the immutable security/compliance trail (may overlap activity but is not identical).

## 94–103. Usage, search, command palette, presence, live views

MVP UX includes usage metering views, global search, command palette, agent/operator presence, and live Computer/Browser observer views (respecting §64 controller rules).

## 104–118. Connectors, connections, grants, API, tokens, encryption

- Split **Connector** (provider type) / **Connection** (account-linked instance) / **Grant** (agent authorization).
- Support native connectors and aggregator connectors.
- Public API is **versioned**.
- Token hierarchy: account → connection → grant → lease (short-lived).
- Credentials encrypted at rest; **no secrets in logs**, tool responses, or activity payloads.

## 119–128. Tenancy and delete semantics

- Account tenancy now; design for future orgs/roles without rewrite.
- Delete semantics are explicit and soft-delete where recovery matters.
- Deleting an **agent** MUST NOT delete account-owned resources (memory, connectors, files, wallet). Resources remain on the account plane.

## 129–134. Onboarding, empty states, design system, a11y, responsive

- First-run onboarding and intentional empty states for every major surface.
- Preserve the original Relay design system language.
- Accessibility baselines (keyboard, contrast, labels) are release requirements.
- Desktop-first; responsive layouts MUST NOT break core workflows.

## 135–139. Observability, correlation, rate limits, abuse

- Correlation IDs on requests, tool calls, and receipts.
- Structured logs/metrics/traces without secret material.
- Rate limits and abuse controls on auth, MCP, connectors, and wallet.

## 140–149. Data model and memory/knowledge pipelines

- Canonical **relational** model for durable domain objects.
- Appropriate stores for blobs, vectors, and event streams.
- Memory and knowledge ingestion pipelines with provenance and retention controls.

## 150–162. MCP, providers, sandbox/computer security

- MCP gateway requirements: dynamic tool projection from capability grants; deny-by-default.
- Provider abstractions isolate SDKs behind Relay ports.
- Sandbox and Computer instances have TTLs, isolation boundaries, and security baselines (no ambient account admin).

## 163–168. Wallet transactions; handoff vs delegation

- Wallet transactions are append-only, idempotent, and policy-checked.
- **Handoff** transfers session continuity between agents.
- **Delegation** grants a subset of authority without transferring identity.
- These are distinct operations and MUST NOT be conflated.

## 169–171. First-run, test connection, demo mode

- First-run guides account → agent → connector → runtime.
- **Test connection** validates a connector without granting agents.
- **Demo mode** is isolated from real customer data and real wallet funds.

## 172–183. Phased delivery (0–10)

Phases 0–10 deliver foundation → identity → capabilities → MCP → memory/connectors/sandbox → dashboard polish → qualification. **Wallet is last.** **Skills arrive late** (after capability/permission engine is solid).

## 184. MVP surface set

MVP includes: Overview, Activity, Agents, Memory, Connectors, Sandbox, Browser, Skills placeholder, Developer/MCP, Permissions, Settings.

Runtime targets for V1 qualification: **Claude Code**, **Codex CLI**, and **custom MCP** clients.

## 185–190. Golden paths

Release golden paths MUST cover:

1. Memory write/read across agents with conflict handling
2. Sandbox execute + file collision behavior
3. Event → wake → inbox
4. Permission deny/allow with lease expiry
5. Performance budgets under concurrent load
6. Security: no secret leakage via MCP/logs/activity

## 191–193. Test strategy

- Unit + contract + integration tests with provider mocks
- Live qualifier accounts for selected connectors (non-prod)
- Evidence retained for Mission Control verification

## 194–198. Acceptance criteria

First release is accepted only when MVP surfaces work, golden paths pass, performance and security qualifications pass, and evidence is durable. Plan approval ≠ acceptance.

## 199. Non-goals

- Not an LLM chat product
- Not a BPM / agent-orchestration framework
- Not a Mission Control / FDLC runtime component
- Not shipping every provider E2E in phase 0
- Not mobile-first
- Not auto-granting capabilities on skill install

## 200. Guardrails

- No Mission Control / FDLC runtime coupling
- MCP ≠ domain model
- No raw credentials to agents
- No one-agent-at-a-time assumption
- No secrets in logs or tool projections

## 201–206. Architecture and north-star experience

Account-owned capability plane beneath replaceable agents. Operator connects once; agents receive grants/leases; every sensitive action leaves a receipt. North-star: swap the agent, keep the world.

## 207. Repository structure

New standalone product layout targets `relay` / `relay-ai` naming. **Current repo remains `jaydubya818/factory-pilot`.** Do **not** move Relay into the MissionControl repository. Mission Control builds Relay; it is not the product monorepo.

## 208. ADRs required

Architecture Decision Records are required for: concurrency/locks, idempotency, connector credential vault, MCP projection, tenancy/delete semantics, wallet sequencing, and demo-mode isolation.

## 209. Objective

Deliver Relay as an account-owned universal capability plane where agents are replaceable and capabilities are durable — with scoped grants/leases, MCP (+ REST/webhooks/events), concurrency-safe resources, receipts/audit, and a desktop dashboard — culminating in golden-path, performance, and security qualification on `jaydubya818/factory-pilot` without Mission Control runtime coupling.

## 210. Official WBS (Mission Plan graph)

| ID | Work order | Notes |
| --- | --- | --- |
| **WO-001** | Repository + Technical Foundation | |
| **WO-002** | Domain Model + Database | |
| **WO-003** | Authentication + Accounts | |
| **WO-004** | Agent Identity + Runtime Connections | |
| **WO-005** | Capability Registry + Permission Engine | |
| **WO-006** | MCP Gateway + Dynamic Tool Projection | |
| **WO-007** | Activity + Audit + Execution Receipts | |
| **WO-008** | Dashboard Shell + Navigation + Design System | |
| **WO-009** | Memory Service + Memory UI | |
| **WO-010** | Connector Framework + First Connector | |
| **WO-011** | Sandbox Service | |
| **WO-012** | Performance Qualification | VALIDATOR / qualification |
| **WO-013** | Security Qualification | VALIDATOR / qualification |
| **WO-014** | Golden-Path E2E Qualification | VALIDATOR / qualification |

**Dependencies:**

- WO-001 → WO-002, WO-003, WO-008
- WO-002 → WO-004
- WO-004 → WO-005
- WO-005 → WO-006, WO-009, WO-010, WO-011
- WO-007 feeds WO-014
- WO-012 + WO-013 → WO-014
- WO-014 also depends on WO-006 through WO-013 as specified for golden-path coverage

## 211–215. First release definition, evidence, DoD, thesis, final directive

- **211 First release:** MVP surfaces (§184) + foundation through sandbox/connector/memory + MCP clients (Claude Code, Codex CLI, custom MCP) + qualifications.
- **212 Evidence:** COMMAND / TEST / BROWSER / PERFORMANCE / SECURITY artifacts retained per assertion; no silent waivers for security/no-secrets/no-MC-coupling.
- **213 Definition of done:** Spec §1–215 encoded; Plan WO-001–WO-014 approved by human operator; all assertions pass with independent verification; golden paths green; wallet still deferred if phase gate says last.
- **214 Thesis:** Agents are replaceable. Capabilities are durable.
- **215 Final directive:** Build the capability plane beneath agents. Do not build another agent. Do not couple to Mission Control at runtime. Do not assume one agent at a time. Prefer receipts over vibes.

---

**Spec completeness marker:** RELAY-V1-SPEC complete through **§215**.
