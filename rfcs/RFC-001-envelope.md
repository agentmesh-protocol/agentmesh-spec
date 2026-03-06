# RFC-001 — AgentMesh Message Envelope

**Status:** Draft  
**Created:** 2025-03  
**Authors:** AgentMesh Contributors  

---

## Summary

This RFC defines the standard message envelope format for all
AgentMesh communications. Every message between two agents
must follow this structure.

---

## Motivation

When two AI agents communicate today, there is no standard format.
One agent might send JSON, another plain text, another a custom
schema. This makes interoperability impossible.

RFC-001 defines the minimal envelope that every AgentMesh message
must contain — regardless of model, framework, or cloud provider.

---

## The Envelope Format
```json
{
  "agentmesh_version": "0.1",
  "id": "msg_a1b2c3d4",
  "from": "agent://alice-f3a2....",
  "to": "agent://bob-c8d1....",
  "timestamp": 1741234567,
  "msg_type": "INTENT",
  "body": {
    "intent_text": "Summarize the Q3 report in 200 words",
    "confidence": 0.91,
    "constraints": {
      "max_latency_ms": 800,
      "language": "en"
    }
  },
  "signature": "ed25519:abc123..."
}
```

---

## Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| agentmesh_version | string | ✅ | Protocol version |
| id | string | ✅ | Unique message ID |
| from | string | ✅ | Sender agent URI |
| to | string | ✅ | Receiver agent URI |
| timestamp | integer | ✅ | Unix timestamp |
| msg_type | string | ✅ | INTENT, RESULT, QUERY, DELEGATION |
| body | object | ✅ | Message payload |
| signature | string | ✅ | Ed25519 signature of the envelope |

---

## Message Types

- **INTENT** — Agent A asks Agent B to do something
- **RESULT** — Agent B returns the outcome of a task
- **QUERY** — Agent A asks for information
- **DELEGATION** — Agent A hands off a full task to Agent B

---

## Agent URI Format

Every agent has a unique URI:
```
agent://{name}-{public_key_prefix}
```

Example:
```
agent://alice-f3a2c8b1
```

The public key prefix is the first 8 characters of the
Ed25519 public key in hex format.

---

## Open Questions

- How do we handle message expiry (TTL)?
- Should `body` be encrypted end-to-end?
- Do we need a `cc` field for broadcasting to multiple agents?

---

## Next RFCs

- RFC-002 — Agent Identity & Key Management
- RFC-003 — Capability Discovery Format
```

---

Commit message:
```
docs: add RFC-001 message envelope format
