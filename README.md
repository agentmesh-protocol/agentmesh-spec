# AgentMesh Protocol

The open communication standard for AI agent-to-agent interaction.

## What is AgentMesh?

AgentMesh defines how AI agents find each other, exchange messages,
verify identity, and collaborate — across any model, any framework,
any cloud.

Think of it as TCP/IP for AI agents.

## Why does this exist?

Today there are millions of AI agents being built with LangChain,
AutoGen, CrewAI, and custom frameworks. None of them can talk to
each other. There is no shared identity layer, no trust protocol,
no standard message format.

AgentMesh fixes that.

## Protocol Layers

| Layer | Name | Status |
|-------|------|--------|
| 1 | Agent Identity & Trust (AITP) | 🟡 Draft |
| 2 | Semantic Message Bus (SMB) | 🟡 Draft |
| 3 | Capability Discovery (CGN) | 🔴 Planned |
| 4 | Agent Economy Layer (AEL) | 🔴 Planned |
| 5 | Orchestration Engine (OCE) | 🔴 Planned |

## RFCs

- [RFC-001 — Message Envelope Format](rfcs/RFC-001-envelope.md)

## Status

Early draft. We are actively looking for contributors and feedback.

Open an Issue to discuss.
