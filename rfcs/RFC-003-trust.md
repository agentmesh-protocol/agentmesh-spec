# RFC-003 - AgentMesh Trust Score Verification

**Status:** Draft  
**Created:** 2026-03  
**Authors:** AgentMesh Contributors  
**Depends on:** RFC-002

## Summary

Defines how trust scores are calculated, updated, and verified.

## Motivation

RFC-002 introduced trust_score as a field in agent records. RFC-003 defines who updates it, how it is verified, and how gaming is prevented.

## Score Changes

Every agent starts with trust_score 0.0.

| Event | Delta |
|-------|-------|
| Task completed | +0.01 |
| Receiving agent confirms quality | +0.02 |
| Task failed or no response | -0.05 |
| Receiving agent reports bad quality | -0.10 |

Score is always capped between 0.0 and 1.0.

## Trust Update Record
```json
{
  "from": "agent://alice-123",
  "about": "agent://bob-456",
  "task_id": "msg_abc123",
  "outcome": "success",
  "quality": "confirmed",
  "timestamp": 1741234567,
  "signature": "ed25519_signature_here"
}
```

## Verification Rules

The gateway verifies each update:
1. Signature is valid
2. from agent exists in registry
3. task_id references a real message
4. outcome is one of: success, failure, timeout
5. quality is one of: confirmed, rejected, neutral

## Anti-Gaming Rules

- An agent cannot submit trust updates about itself
- Trust updates older than 24 hours are rejected
- Maximum 10 trust updates per agent per hour
- New agents with trust_score 0.0 cannot submit updates

## Trust Tiers

| Score | Tier | Meaning |
|-------|------|---------|
| 0.0 - 0.2 | Unverified | New or untrusted agent |
| 0.2 - 0.5 | Basic | Some track record |
| 0.5 - 0.8 | Trusted | Reliable agent |
| 0.8 - 1.0 | Elite | Highly trusted agent |

## Next RFCs

- RFC-004 Agent-to-Agent HTTP Transport
- RFC-005 Agent Economy and Micro-Transactions
```

Commit message:
```
docs: add RFC-003 trust score verification
