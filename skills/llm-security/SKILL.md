---
name: llm-security
description: "Use this skill to secure AI agents and LLM applications. Helps with: mitigating prompt injection, auditing AI architecture, securing tool calling, setting up security gateways, evaluating RAG safety, detecting jailbreaks, and implementing observability. Apply this whenever you're building AI agents, securing LLM outputs, or addressing LLM vulnerabilities."
category: security
risk: safe
source: community
tags: [llm-security, prompt-injection, ai-safety, agent-engineering, owasp-llm]
---

# LLM Security

## Core Principles
- Assume the model is compromised - don't rely on "security by incompetence"
- Defense in depth - layer security at input (gateways), tools (contracts), data (access control), and output (filters)
- Least privilege - agents only get capabilities they need

## Key Domains

### 1. System Architecture
- Run agents in isolated containers with restricted network
- Implement human-in-the-loop (HITL) for sensitive actions (payments, PII access, deletes)
- Log all tool calls and chain-of-thought reasoning

### 2. Tool Contracts
- Use strongly-typed schemas, not generic strings
- Add regex patterns for identifiers (e.g., `^USR_[0-9]{6}$`)
- Include usage examples in tool descriptions to anchor behavior
- Add rate limits and execution timeouts

Example:
```json
{
  "name": "transfer_funds",
  "input_schema": {
    "from_account": { "type": "string", "pattern": "^ACC_[A-Z0-9]{8}$" },
    "amount": { "type": "number", "minimum": 0.01, "maximum": 1000000 }
  },
  "requires_human_approval": true,
  "rate_limit": "5 calls/min"
}
```

### 3. Security Gateway (Input/Output Filtering)
Input Gateway:
- Detect prompt injection (block "ignore previous", "system prompt", "override")
- Classify intent (harmful vs legitimate)
- Validate external data sources (if RAG)

Output Gateway:
- Redact PII (emails, phones, SSNs, API keys)
- Detect jailbreaks (filter if model violated guidelines)
- Sanitize code (scan for dangerous imports/functions)
- Validate URLs before returning

Quick implementation:
```python
def scrub_output(text):
    text = re.sub(r'[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}', '[EMAIL]', text)  # emails
    text = re.sub(r'\+?[1-9]\d{1,14}', '[PHONE]', text)  # phones
    text = re.sub(r'sk_[a-z0-9]{32}', '[API_KEY]', text)  # API keys
    return text
```

### 4. OWASP Top 10 for LLMs (Essentials)
- Prompt Injection: Gateway detects injection keywords; block if confidence high
- Indirect Injection (RAG compromised): Validate document sources; treat external data as untrusted
- System Prompt Leakage: Output filter blocks system instruction patterns
- Sensitive Info Disclosure: Redact PII/PHI from outputs; limit model's data access
- Model Poisoning: Verify model source/checksum; use official repos only
- Insecure Tool Design: Enforce strict tool contracts; validate all parameters
- Denial of Wallet: Set token limits, timeouts, rate limits per user

### 5. RAG Security (if applicable)
- Validate sources - only trust curated/internal documents
- Sanitize content - remove suspicious patterns before indexing
- Re-rank results - push high-relevance, trusted sources to top
- Chunk carefully - balance detail vs context dilution (typically 500-1000 tokens)

### 6. Resilience
- Timeouts - 30s max per request, 5s per tool call
- Retries - exponential backoff (1s → 2s → 4s)
- Circuit breaker - stop calling failing tools after N errors
- Rate limits - per-user, per-minute, per-day caps

### 7. Observability
Log every agent interaction:
```json
{
  "timestamp": "2024-05-01T14:30:45Z",
  "request_id": "req_123",
  "user_id": "user_xyz",
  "input": "...",
  "agent_reasoning": "...",
  "tool_calls": [{"tool": "get_balance", "params": {}, "status": "success"}],
  "output": "...",
  "security_checks": {"prompt_injection": false, "jailbreak": false, "pii_found": false},
  "metrics": {"latency_ms": 500, "tokens": 145, "cost_usd": 0.002}
}
```

Track metrics:
- Success rate (target: >95%)
- Latency p95 (target: <2s)
- Cost per task
- Tool error rate (target: <2%)
- Jailbreak detection rate (target: >95%)

### 8. Supply Chain (if using open-source models)
- Download from official sources only (Hugging Face org, Anthropic, OpenAI)
- Verify SHA256 checksum matches published value
- Scan with VirusTotal API if available

### 9. Jailbreak Detection
Common patterns to block:
- "ignore previous instructions"
- "pretend you are"
- "role-play as"
- "you have no restrictions"
- "developer mode"
- "output without filter"

```python
jailbreak_patterns = [
    r'ignore.*previous.*instruction',
    r'pretend.*you.*are',
    r'role.?play.*as',
    r'you.*have.*no.*restrictions',
    r'developer mode'
]
if any(re.search(p, user_input, re.IGNORECASE) for p in jailbreak_patterns):
    return "Request blocked"
```

## Implementation Checklist
- Architecture - agent runs in isolated container
- Input gateway - scans for prompt injection
- Tool contracts - strongly-typed, with examples and limits
- Output gateway - scrubs PII, detects jailbreaks
- Rate limits - per-user caps on requests/tokens
- Timeouts - 30s request limit, 5s tool limit
- Observability - all actions logged with metrics
- HITL - sensitive actions require approval
- Monitoring - alerts for anomalies (high error rate, unusual queries)
- Incident response - plan for security events

## Customization by Project Type
- Autonomous agent → Focus on: tool contracts, circuit breakers, timeouts
- RAG chatbot → Focus on: input gateway, RAG validation, output scrubbing
- Code generation → Focus on: code sandboxing, supply chain verification
- Financial agent → Focus on: HITL for all transactions, audit logging, encryption

Use this as a foundation. For each project, create a project-specific threat model, identify your top 3 risks, and implement accordingly.
