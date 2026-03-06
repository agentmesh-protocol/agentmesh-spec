# RFC-002 — AgentMesh Agent Registry

**Status:** Draft  
**Created:** 2025-03  
**Authors:** AgentMesh Contributors  
**Depends on:** RFC-001

## Summary

Defines how agents register themselves and discover other agents.

## Registration Record
```json
{
  "uri": "agent://bob-e9c6d340",
  "name": "bob",
  "capabilities": ["text_summarization", "translation"],
  "model": "claude-sonnet-4-6",
  "trust_score": 0.0,
  "registered_at": 1741234567,
  "ttl": 3600
}
```

## Discovery Query
```json
{
  "query_type": "capability",
  "capability": "text_summarization",
  "min_trust_score": 0.5,
  "limit": 5
}
```

## Trust Score

- Starts at 0.0 for every new agent
- Task completed successfully: +0.01
- Receiving agent confirms quality: +0.02
- Task failed: -0.05
- No response within TTL: -0.02
- Capped between 0.0 and 1.0

## Storage

Cloudflare KV:
- Key: agent:{uri}
- Value: Registration record as JSON
- TTL: Set by agent, max 86400 seconds
```

---

Commit message:
```
docs: add RFC-002 agent registry
