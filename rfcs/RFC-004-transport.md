# RFC-004 - AgentMesh Agent-to-Agent HTTP Transport

**Status:** Draft
**Created:** 2026-03
**Authors:** AgentMesh Contributors
**Depends on:** RFC-001, RFC-002, RFC-003

## Summary

Defines how agents communicate directly over HTTP without routing through the central gateway.

## Motivation

RFC-001 defines the message format. RFC-002 defines discovery. But how does Agent A actually deliver a message to Agent B directly?

Today all messages route through the AgentMesh Gateway. This works but creates a bottleneck. RFC-004 enables direct peer-to-peer HTTP transport between agents.

## Agent Endpoint

Every agent that supports direct transport must expose an HTTP endpoint:

POST /agentmesh/v1/receive

The agent announces this endpoint when registering:

```json
{
  "uri": "agent://bob-456",
  "name": "bob",
  "capabilities": ["summarization"],
  "endpoint": "https://my-agent.example.com/agentmesh/v1/receive",
  "trust_score": 0.87
}
```

## Request Format

The sender POSTs a signed RFC-001 message envelope to the receiver endpoint:

```json
{
  "agentmesh_version": "0.1",
  "id": "msg_abc123",
  "from": "agent://alice-123",
  "to": "agent://bob-456",
  "timestamp": 1741234567,
  "msg_type": "INTENT",
  "body": {
    "intent_text": "Summarize the Q3 report",
    "confidence": 1.0
  },
  "signature": "ed25519_signature_here"
}
```

## Response Format

The receiver responds synchronously:

```json
{
  "status": "accepted",
  "message_id": "msg_abc123",
  "reply": {
    "agentmesh_version": "0.1",
    "id": "msg_reply_xyz",
    "from": "agent://bob-456",
    "to": "agent://alice-123",
    "timestamp": 1741234568,
    "msg_type": "RESPONSE",
    "body": {
      "intent_text": "The Q3 report shows 23% growth...",
      "confidence": 0.95
    },
    "signature": "ed25519_signature_here"
  }
}
```

## Verification

Before processing any message, the receiver must:

1. Check agentmesh_version is supported
2. 2. Verify the signature using the sender public key from the registry
   3. 3. Check the timestamp is within 300 seconds of current time
      4. 4. Check the to field matches the receiver URI
        
         5. If any check fails, return HTTP 401 with error details.
        
         6. ## Transport Rules
        
         7. - HTTP/HTTPS only
            - - T# RFC-004 - AgentMesh Agent-to-Agent HTTP Transport
             
              - **Status:** Draft
              - **Created:** 2026-03
              - **Authors:** AgentMesh Contributors
              - **Depends on:** RFC-001, RFC-002, RFC-003
             
              - ## Summary
             
              - Defines how agents communicate directly over HTTP without routing through the central gateway.
             
              - ## Motivation
             
              - RFC-001 defines the message format. RFC-002 defines discovery. But how does Agent A actually deliver a message to Agent B directly?
             
              - Today all messages route through the AgentMesh Gateway. This works
