# 🔐 LLM Security & AI Agent Hardening

## Overview

This skill provides security patterns for AI-integrated applications, particularly those using LLMs with tool calling, RAG, and autonomous agents. It is **applied once per project** and customized to threat profile.

## Core Principles

1. **Assume the model is compromised** – Don't rely on "security by incompetence." Treat LLM output as untrusted.
2. **Defense in Depth** – Layer security across input validation, tool contracts, data access, and output filtering.
3. **Least Privilege** – Agents get only capabilities they need. Tools have minimal permissions.
4. **Fail Secure** – On doubt, block. On failure, log and escalate.

---

## Domain Breakdown

### 1. System Architecture

**Principle**: Run agents in isolated environments with restricted capabilities.

**Checklist**:
- [ ] Agent runs in isolated container (Docker, Kubernetes)
- [ ] Network egress restricted (only trusted domains)
- [ ] All tool calls logged with full parameters
- [ ] Chain-of-thought reasoning captured for forensics
- [ ] HITL required for sensitive actions (payments, PII access, deletes)

**Implementation**:
```
User Input → [Input Gateway] → [Agent + Tools] → [Output Gateway] → User
```

---

### 2. Tool Contracts & Execution Safety

**Principle**: Strongly-typed schemas prevent "LLM Imagination" (model inventing parameters).

**Template**:
```json
{
  "name": "transfer_funds",
  "description": "Transfer money. REQUIRES APPROVAL for amounts > $10,000.",
  "input_schema": {
    "type": "object",
    "properties": {
      "from_account": {
        "type": "string",
        "pattern": "^ACC_[A-Z0-9]{8}$",
        "description": "Source account (format: ACC_AB12345C)"
      },
      "amount": {
        "type": "number",
        "minimum": 0.01,
        "maximum": 1000000
      }
    },
    "required": ["from_account", "amount"],
    "additionalProperties": false
  },
  "requires_human_approval": true,
  "approval_threshold_usd": 10000,
  "rate_limit": "5 calls/min",
  "timeout_seconds": 30
}
```

**Best Practices**:
- Use enums for fixed-set values (not strings)
- Add regex patterns for all identifiers
- Include 2-3 "known good" examples in description
- Define rate limits and execution timeouts
- Require HITL for high-impact operations

---

### 3. Input Gateway (Prompt Injection Defense)

**Threats**: Direct prompt injection ("ignore previous instructions"), indirect injection (compromised RAG data).

**Defense**:
```python
def detect_prompt_injection(user_input):
    patterns = [
        r'ignore.*previous.*instruction',
        r'forget.*previous.*instruction',
        r'system prompt',
        r'override.*instruction',
        r'disregard.*guideline'
    ]
    return any(re.search(p, user_input, re.IGNORECASE) for p in patterns)

if detect_prompt_injection(user_input):
    return "Request blocked: Potential prompt injection detected"
```

**Advanced**: Use LlamaGuard (Meta's safety classifier) for higher accuracy.

---

### 4. Output Gateway (PII Redaction & Jailbreak Detection)

**Defense**: Treat LLM output as untrusted user input.

```python
def scrub_output(text):
    # PII redaction
    text = re.sub(r'[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}', '[EMAIL]', text)
    text = re.sub(r'\+?[1-9]\d{1,14}', '[PHONE]', text)
    text = re.sub(r'sk_[a-z0-9]{32}', '[API_KEY]', text)
    return text

def detect_jailbreak_output(text):
    """Check if model violated safety guidelines."""
    harmful_patterns = [
        r'how to.*bomb',
        r'how to.*poison',
        r'step.?by.?step.*illegal'
    ]
    return any(re.search(p, text, re.IGNORECASE) for p in harmful_patterns)
```

**Checklist**:
- [ ] PII redacted (emails, phones, SSNs, API keys, internal IPs)
- [ ] System prompt patterns blocked from output
- [ ] Generated code scanned for dangerous imports (`eval`, `exec`, `os.system`)
- [ ] URLs validated before returning (check VirusTotal if available)

---

### 5. RAG Security (if applicable)

**Threats**: Poisoned documents, sensitive data leakage, indirect prompt injection.

**Defense Layers**:

1. **Source Validation** – Only trust curated/internal sources
2. **Content Sanitization** – Remove suspicious patterns before indexing
3. **Re-ranking** – Push high-relevance, trusted sources to top
4. **Chunking** – Balance detail vs. context (500–1000 tokens typical)

```python
def validate_rag_source(document_text):
    suspicious = [r'ignore.*instruction', r'system prompt', r'hidden.*instruction']
    return not any(re.search(p, document_text, re.IGNORECASE) for p in suspicious)

def rerank_chunks(query, chunks):
    """Score by relevance + trustworthiness."""
    ranked = []
    for chunk in chunks:
        relevance = semantic_similarity(query, chunk)  # 0–1
        safety = 0.9 if validate_rag_source(chunk) else 0.1
        score = relevance * safety
        ranked.append((chunk, score))
    return sorted(ranked, key=lambda x: x[1], reverse=True)[:5]
```

---

### 6. Resilience & Resource Management

**Threats**: Denial-of-wallet (token exhaustion), cascading failures.

**Defenses**:
- **Timeouts**: 30s per request, 5s per tool call
- **Retries**: Exponential backoff (1s → 2s → 4s)
- **Circuit Breaker**: Stop calling failing tools after N errors
- **Rate Limits**: Per-user, per-minute, per-day caps
- **Token Budget**: Hard cap on tokens per request/user

```python
def with_timeout(timeout_seconds):
    def decorator(func):
        @wraps(func)
        async def wrapper(*args, **kwargs):
            return await asyncio.wait_for(func(*args, **kwargs), timeout=timeout_seconds)
        return wrapper
    return decorator

@with_timeout(30)
async def call_llm(prompt):
    return await client.messages.create(
        model="claude-opus-4-6",
        max_tokens=1000,  # Hard cap
        messages=[{"role": "user", "content": prompt}]
    )
```

---

### 7. OWASP Top 10 for LLMs (Quick Reference)

| # | Threat | Counter-measure |
| --- | --- | --- |
| 1 | **Direct Prompt Injection** | Input gateway detects injection keywords |
| 2 | **Indirect Injection** (RAG poisoned) | Validate doc sources; re-rank by trustworthiness |
| 3 | **System Prompt Leakage** | Output filter blocks system instruction patterns |
| 4 | **Sensitive Info Disclosure** | PII redaction; limit data access scope |
| 5 | **Model Inversion** (extract training data) | Monitor for suspicious query patterns; throttle |
| 6 | **Model Poisoning** | Verify model source/checksum; use official repos |
| 7 | **Supply Chain** | Scan dependencies; use lock files; audit open-source |
| 8 | **Insecure Plugin Design** | Enforce tool contracts; sandbox execution |
| 9 | **Unauthorized Tool Use** | Whitelist tools; check permissions per user |
| 10 | **Denial of Wallet** | Token limits, timeouts, rate limits, circuit breakers |

---

### 8. Supply Chain & Model Verification

**For open-source models**:

```python
class ModelValidator:
    def validate_model(self, model_name, model_path):
        # 1. Verify source (official Hugging Face org, Anthropic, OpenAI)
        # 2. Verify SHA256 checksum
        # 3. Scan with VirusTotal API (optional)
        # 4. Check model card for training data, known issues
        # 5. Pin exact version (never use "latest")
        pass
```

---

### 9. Observability & Metrics

**Log every interaction**:
```json
{
  "timestamp": "2024-05-01T14:30:45Z",
  "request_id": "req_123",
  "user_id": "user_xyz",
  "input": "query",
  "agent_reasoning": "internal thoughts",
  "tool_calls": [{"tool": "get_balance", "params": {...}, "status": "success"}],
  "output": "response",
  "security_checks": {
    "prompt_injection": false,
    "jailbreak": false,
    "pii_found": false
  },
  "metrics": {"latency_ms": 500, "tokens": 145, "cost_usd": 0.002}
}
```

**Key Metrics**:
- Success rate (target: >95%)
- Latency p95 (target: <2s)
- Cost per task
- Tool error rate (target: <2%)
- Jailbreak detection rate (target: >95%)

---

### 10. Incident Response & Auditing

**Sensitive Actions Require HITL**:
- Financial transactions
- PII/PHI access
- Data deletion
- Privilege escalation
- System configuration changes

**Maintain audit trails**:
- All tool calls logged with user + timestamp
- Chain-of-thought reasoning recorded
- Security events (injection attempts, jailbreaks) escalated

---

## Per-Project Customization

### For Autonomous Agents
Focus: Tool contracts, circuit breakers, timeouts, HITL for critical actions

### For RAG Chatbots
Focus: Input gateway, RAG validation, output scrubbing, PII redaction

### For Code Generation
Focus: Code sandboxing, AST analysis for dangerous patterns, supply chain verification

### For Financial Agents
Focus: HITL for all transactions, audit logging, encryption at rest

---

## Implementation Workflow

1. **Identify Threats** – Use OWASP Top 10 checklist; focus on top 3 for your project
2. **Implement Gateways** – Input (injection detection) + Output (PII, jailbreak)
3. **Harden Tools** – Strongly-typed contracts, rate limits, timeouts
4. **Add Observability** – Log all interactions; set up monitoring
5. **Test & Iterate** – Red team; measure metrics; refine

---

## Checklists

### Pre-Launch Security Checklist
- [ ] Input gateway (prompt injection detection) implemented
- [ ] Output gateway (PII redaction, jailbreak detection) implemented
- [ ] Tool contracts (strongly-typed, rate limits, timeouts) defined
- [ ] HITL workflow for sensitive actions implemented
- [ ] Observability logging in place
- [ ] Resilience (timeouts, retries, circuit breaker) configured
- [ ] Rate limits enforced per user
- [ ] Incident response plan documented
- [ ] Monitoring & alerting set up
- [ ] Security testing (red team) completed

### Ongoing Security Checklist
- [ ] Weekly: Review security logs for anomalies
- [ ] Monthly: Audit tool access patterns
- [ ] Quarterly: Red team exercise
- [ ] Yearly: Full security assessment
