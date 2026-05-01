# 🧠 Universal Engineering Skill Bundle

This document defines the mandatory architectural, security, and quality standards for development. Use these skills to guide code generation, refactors, and system reviews.

> **Usage:** Invoke a skill by name. For example: *"Review this pull request using the **ArchitectureDesign** and **SecurityDevSecOps** skills."*

---

## 🔍 Skill: AdaptiveProjectAnalysis
*Analysis-first architectural skill to evaluate, validate, and conditionally refactor project structures.*

* **Core Principle**: Architecture exists to reduce the cost of change; if the cost of change is already low, do nothing.
* [cite_start]**Mandatory Analysis (Phase 1)**: Before recommending changes, analyze language (Python/JS/TS), project type, architecture signals (Clean, Hexagonal, DDD), and project maturity (Prototype vs. Production)[cite: 121, 132, 133, 134, 135].
* **Structural Health Check (Phase 2)**:
    * 🟢 **Green — Healthy**: Clear separation of layers, isolated frontend/backend, and centralized config. [cite_start]**Action**: Validate and generate documentation only[cite: 43, 224].
    * 🟡 **Yellow — Advisory**: Minor mixing of concerns or monorepo without strict boundaries. **Action**: Suggest incremental improvements without enforced refactors.
    * 🔴 **Red — Refactor Required**: Mixed frontend/backend, hardcoded secrets, or business logic inside UI/Controllers. [cite_start]**Action**: Propose a staged, low-risk refactor plan[cite: 32, 33, 126].
* [cite_start]**Compatibility Rule**: If the project already follows Clean Architecture, Hexagonal Architecture, or DDD, validate and document only; never override existing architecture-enforcing skills[cite: 12, 13, 47].
* [cite_start]**Hard Constraints**: Changes must be incremental and reversible; prefer moving files over rewriting logic[cite: 39, 120].
* [cite_start]**Documentation Requirement**: Always maintain or update `docs/architecture.md`, `decisions.md` (ADRs), and `conventions.md`[cite: 28, 44, 49, 57].
* [cite_start]**Docstring Standards**: All public-facing code must have JSDoc (JS), TSDoc (TS), or Docstrings (Python) explaining **intent**, not implementation details[cite: 51, 146].



## 🏗️ Skill: ArchitectureDesign
*Maintain a clean, modular system that separates business intent from technical implementation.*

* [cite_start]**Modular Monolith**: Implement a Modular Monolith using Hexagonal (Clean) Architecture patterns[cite: 12].
* [cite_start]**Domain Purity**: The Domain Layer must remain "pure" and independent of any framework, database, or AI SDK[cite: 13, 43].
* [cite_start]**Separation of Concerns (SRP)**: Strictly separate domain logic from Infrastructure (data access) and Presentation (UI) layers[cite: 14, 48].
* [cite_start]**Functional Grouping**: Organize code by functional domain (e.g., Requirements, Validation) rather than just technical layers[cite: 15].
* [cite_start]**SOLID Compliance**: Adhere to SOLID principles, ensuring each class or module has a single reason to change[cite: 42, 48, 298, 299].
* [cite_start]**Dependency Inversion (DIP)**: Ensure high-level modules depend on abstractions (interfaces), not low-level implementations[cite: 315, 316, 317, 318].

## 🛡️ Skill: SecurityDevSecOps
*Integrate security into every layer of the software development lifecycle.*

* [cite_start]**Strict Validation**: Validate all external data and API inputs using strict schemas like Zod or Valibot[cite: 52, 126, 241, 264].
* [cite_start]**OWASP for LLMs**: Implement specific defenses against Prompt Injection and prevent sensitive data leakage into logs[cite: 56, 249, 250, 272, 273].
* [cite_start]**Injection Defense**: Use parameterized logic and avoid dynamic string templates or `eval()` for commands and queries[cite: 89, 246, 265].
* [cite_start]**Secure Headers**: Implement **Helmet.js** to enforce critical security headers like Content-Security-Policy (CSP)[cite: 56, 90, 247, 268].
* [cite_start]**Secret Management**: Never commit credentials; use `.env` files validated at runtime and scan for secrets using tools like GitLeaks[cite: 85, 253, 269].

## 🧪 Skill: QualityTesting (Honest Coverage & Automation)
*Prioritize real logic over boilerplate using a tiered strategy and automated enforcement.*

* [cite_start]**100/80/0 Strategy**: Apply **100% coverage** to CORE (domain logic), **80% coverage** to GLOBAL (application/UI), and **0% coverage** to INFRA (static config)[cite: 139, 140, 141, 176, 180, 185].
* [cite_start]**Automated Quality Gates (Husky)**: Implement a mandatory `pre-push` hook using Husky to block any code that fails the test suite or falls below the 80% coverage threshold[cite: 66, 69, 148, 325].
* **Mac-Compatible Setup**: Always ensure the hook is executable by running `chmod +x .husky/pre-push` during project setup.
* [cite_start]**Deterministic Mocking**: All external AI and infrastructure services must be mocked to ensure consistent, cost-effective results[cite: 331, 332].
* [cite_start]**Traceability Validation**: Every AI-generated suggestion must include textual evidence (source excerpts) and be recorded in a `DecisionLog`[cite: 20, 154, 274, 338].
* [cite_start]**Code Smell Detection**: Regularly audit for "God Objects," Magic Numbers, and code duplication to maintain maintainability[cite: 32, 33, 34, 145].



## 🔄 Skill: ResilientLogic
*Design for stability in distributed and non-deterministic AI environments.*

* [cite_start]**Fault Tolerance**: Implement Timeouts, Retries, and **Circuit Breakers** for all external AI API calls[cite: 17].
* [cite_start]**Statelessness**: Design services to be stateless to facilitate horizontal scaling[cite: 18].
* [cite_start]**Idempotency**: Ensure processing the same input twice does not result in inconsistent states[cite: 19].
* [cite_start]**Graceful Degradation**: Provide fallbacks or "Ambiguous" statuses if a processing step fails, avoiding total system failure[cite: 102, 114, 339].
* [cite_start]**Explicit Error Types**: Define custom, testable domain error classes (e.g., `ValidationError`) rather than generic catches[cite: 53, 101, 128].

## 🤝 Skill: UniversalUX
*Create accessible, transparent, and responsive user interfaces.*

* [cite_start]**Visibility of Status**: Use progress indicators and "Thinking..." animations during long operations[cite: 110, 161, 369].
* [cite_start]**Actionable Feedback**: Provide human-readable error messages with clear paths to resolution[cite: 111, 353, 371].
* [cite_start]**Accessibility (WCAG 2.1 AA)**: Maintain full keyboard navigation, high-contrast text, and descriptive ARIA labels[cite: 170, 373, 374, 377].
* [cite_start]**Optimistic UI**: Ensure feedback for user interactions occurs in **<100ms**, with automatic rollbacks on failure[cite: 112, 288, 379].
* [cite_start]**Human-in-the-loop**: The system acts as a "Copilot"; the human user always makes the final call[cite: 23, 60, 251].

## 🧩 Skill: DDDImplementation (Domain-Driven Design)
*Expert-level guidance shifting focus from data structures to business models and behaviors.*

* **Design by Domain vs. Data**: Avoid "Anemic Domain Models." Do not start with DB tables or IDs. Focus on logic first, persistence last.
* **Ubiquitous Language**: Code must reflect business terminology. Use methods like `.enroll()` or `.approve()` instead of generic `.updateStatus()` or `.insertRow()`.
* **Tactical Building Blocks**:
    * **Value Objects**: Eliminate "Primitive Obsession" (e.g., use an `Email` class instead of a `string`). Ensure they are immutable and self-validating.
    * **Aggregates**: Group related entities. Use an **Aggregate Root** to protect business invariants (e.g., ensuring a "Course" never exceeds capacity).
* **Strategic Separation**:
    * **Application Services**: Act as orchestrators only. Fetch data, trigger domain logic, and save. No business logic or HTTP/framework code allowed here.
    * **Ports & Adapters**: Define interfaces in the Domain; implement persistence/APIs in the Infrastructure layer.
* **Decoupled Persistence**: Use explicit **Mappers** to separate domain entities from database schemas. Use `toDomain()` for reconstruction.
* **Refactoring Path**: Avoid "Big Bang" rewrites. Start by identifying primitive data and refactoring it into Value Objects.

## 🔐 Skill: LLMSecurityAI
*Secure AI agents and LLM applications against prompt injection, jailbreaks, and tool misuse.*

* **Assume Compromise**: Treat the model as a potential vulnerability. Do not rely on "security by incompetence."
* **Defense in Depth**: Layer security at input (gateways), tools (contracts), data (access control), and output (filters).
* **Least Privilege**: Grant agents only the capabilities they require. Scope tool permissions tightly.
* **Tool Contract Enforcement**: Use strongly-typed schemas (not generic strings). Add regex patterns for identifiers (e.g., `^USR_[0-9]{6}$`), usage examples, rate limits, and execution timeouts.
* **Input Gateway**: Detect and block prompt injection patterns ("ignore previous", "system prompt", "override"). Classify user intent (harmful vs. legitimate). Validate external data sources.
* **Output Gateway**: Redact PII (emails, phones, SSNs, API keys). Detect jailbreaks (filter if model violated guidelines). Sanitize code (block dangerous imports/functions). Validate URLs.
* **RAG Safety** (if applicable): Validate document sources (trust only curated/internal). Sanitize content before indexing. Re-rank results by relevance + trustworthiness. Chunk thoughtfully (500–1000 tokens).
* **Resilience**: Implement timeouts (30s request, 5s tool), exponential backoff retries (1s → 2s → 4s), circuit breakers (stop after N failures), and per-user rate limits.
* **Observability**: Log all agent interactions (query, reasoning, tool calls, output, metrics). Track: success rate (>95%), latency p95 (<2s), cost per task, tool error rate (<2%), jailbreak detection (>95%).
* **Supply Chain** (open-source models): Download from official sources only. Verify SHA256 checksum. Scan with VirusTotal if available.
* **OWASP Top 10 for LLMs**: Mitigate prompt injection, indirect injection (RAG), system prompt leakage, sensitive info disclosure, model poisoning, insecure tool design, and denial-of-wallet attacks.
* **Sensitive Action HITL**: Require human approval for payments, PII access, data deletes, and critical system changes. Maintain audit trails.
* **Jailbreak Patterns**: Block common patterns: "ignore previous", "role-play as", "pretend you are", "developer mode", "output without filter".

---