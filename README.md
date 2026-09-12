# Relay (factory-pilot)

**Relay** is an AI Agent Infrastructure product: a **Universal Capability Plane**.

> Agents are replaceable. Capabilities are durable.

This repository is the Relay product source. It is **built by** [Mission Control](https://github.com/jaydubya818/MissionControl) Software Factory governance (`Mission → Plan → WorkOrder → verify → accept`), but Relay is **not** a Mission Control runtime dependency and must not import MC/FDLC packages.

## Promise

Give any AI agent superpowers. Connect your digital world once. Agents can change; capabilities don't.

Relay is a persistent capability / identity / state / execution / event / integration layer beneath agents — not an LLM, chatbot, coding agent, factory, or orchestration framework.

## Spec

Operator-facing V1 Mission Spec: [`docs/RELAY-V1-SPEC.md`](./docs/RELAY-V1-SPEC.md)

## First milestone

Scaffold a Next.js App Router TypeScript app with:

- `/api/health`
- Desktop-first dashboard shell (PLANE / CAPABILITIES / AUTOMATION / DEVELOPER / CONTROL)
- Domain models for the full capability plane object set
- MCP gateway stub (V1 agent protocol; not the internal domain model)

## Status

- Visibility: public
- Factory project slug: `relay`
- Intake: Mission Spec in local Mission Control (see operator notes)

## Links

- Product context (not dependencies): [fdlc.ai](https://fdlc.ai/) · [Mission Control product page](https://fdlc.ai/mission-control)
