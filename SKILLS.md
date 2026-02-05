# 🧠 Universal Engineering Skill Bundle

This document defines the mandatory architectural, security, and quality standards for development. Use these skills to guide code generation, refactors, and system reviews.

> **Usage:** Invoke a skill by name. For example: *"Review this pull request using the **ArchitectureDesign** and **SecurityDevSecOps** skills."*

---

## 🏗️ Skill: ArchitectureDesign
*Maintain a clean, modular system that separates business intent from technical implementation.*

* [cite_start]**Modular Monolith:** Implement a Modular Monolith using Hexagonal (Clean) Architecture patterns[cite: 12].
* [cite_start]**Domain Purity:** The Domain Layer must remain "pure," independent of frameworks, databases, or external AI SDKs[cite: 13, 43].
* [cite_start]**Separation of Concerns (SRP):** Strictly separate domain logic from Infrastructure (data access) and Presentation (UI) layers[cite: 14, 48].
* [cite_start]**Functional Grouping:** Organize code by functional domain rather than just technical layers[cite: 15].
* [cite_start]**SOLID Compliance:** Adhere to SOLID principles, ensuring each class or module has a single reason to change [cite: 42, 298-299].
* [cite_start]**Dependency Inversion (DIP):** Ensure high-level modules depend on abstractions (interfaces), not low-level implementations [cite: 315-318].

## 🛡️ Skill: SecurityDevSecOps
*Integrate security into every layer of the software development lifecycle.*

* [cite_start]**Strict Validation:** Validate all external data and API inputs using strict schemas like Zod or Valibot[cite: 52, 241, 264].
* [cite_start]**OWASP for LLMs:** Implement specific defenses against Prompt Injection and prevent sensitive data leakage into logs or external training sets [cite: 249, 250, 272-273].
* [cite_start]**Injection Defense:** Use parameterized logic and avoid dynamic string templates or `eval()` for commands and queries[cite: 89, 246, 265].
* [cite_start]**Secure Headers:** Implement **Helmet.js** to enforce critical security headers like Content-Security-Policy (CSP)[cite: 56, 90, 247, 268].
* [cite_start]**Secret Management:** Never commit credentials; use `.env` files validated at runtime and scan for secrets using tools like GitLeaks[cite: 85, 253, 269].

## 🧪 Skill: QualityTesting (Honest Coverage)
*Prioritize real logic over boilerplate using a tiered "Honest Coverage" strategy.*

* [cite_start]**100/80/0 Strategy:** Apply **100% coverage** to CORE (domain logic), **80% coverage** to GLOBAL (application/UI), and **0% coverage** to INFRA (static config) [cite: 138-141].
* [cite_start]**Pre-push Enforcement:** Use **Husky** and Git hooks to block any code that fails tests or does not meet coverage thresholds[cite: 69, 148, 198].
* [cite_start]**Deterministic Mocking:** Mock all external AI and infrastructure services to ensure consistent, cost-effective test results[cite: 331, 332].
* [cite_start]**Auditability:** Every system decision must be linked to a `DecisionLog` and specific textual evidence from source documents[cite: 20, 154, 338].
* [cite_start]**Code Smell Detection:** Audit regularly for "God Objects," Magic Numbers, and code duplication [cite: 32-34, 145].
* 
🧪 Skill: QualityTesting (Honest Coverage & Automation)
Prioritize real logic over boilerplate using a tiered strategy and automated enforcement.


100/80/0 Strategy: Apply 100% coverage to CORE (domain logic), 80% coverage to GLOBAL (application/UI), and 0% coverage to INFRA (static config/mocks) .


Automated Quality Gates (Husky): Implement a mandatory pre-push hook using Husky to block any code that fails the test suite or falls below the 80% global coverage threshold .


Mac-Compatible Setup: Always ensure the hook is executable by running chmod +x .husky/pre-push during the setup phase.


Deterministic Mocking: All external AI and infrastructure services (OpenAI, PDF Parsers) must be mocked to ensure consistent, cost-effective test results.


Traceability Validation: Verify that every AI-generated suggestion includes textual evidence and is recorded in a DecisionLog.


Code Smell Detection: Regularly audit for "God Objects," Magic Numbers, and code duplication to maintain high maintainability scores .

## 🔄 Skill: ResilientLogic
*Design for stability in distributed and non-deterministic AI environments.*

* [cite_start]**Fault Tolerance:** Implement Timeouts, Retries, and **Circuit Breakers** for all external AI API calls[cite: 17].
* [cite_start]**Statelessness:** Design services to be stateless to facilitate horizontal scaling[cite: 18].
* [cite_start]**Idempotency:** Ensure processing the same input twice does not result in inconsistent states[cite: 19].
* [cite_start]**Graceful Degradation:** Provide fallbacks or "Ambiguous" statuses if a processing step fails, avoiding total system failure[cite: 102, 114, 339].
* [cite_start]**Explicit Error Types:** Define custom, testable domain error classes (e.g., `ValidationError`, `ExtractionError`) rather than generic catches[cite: 53, 101].

## 🤝 Skill: UniversalUX
*Create accessible, transparent, and responsive user interfaces.*

* [cite_start]**Visibility of Status:** Use progress indicators and "Thinking..." animations during long operations[cite: 110, 161, 369].
* [cite_start]**Actionable Feedback:** Provide human-readable error messages with clear paths to resolution[cite: 111, 353, 371].
* [cite_start]**Accessibility (WCAG 2.1 AA):** Maintain full keyboard navigation, high-contrast text, and descriptive ARIA labels [cite: 170, 373-377].
* [cite_start]**Optimistic UI:** Ensure feedback for user interactions occurs in **<100ms**, with automatic rollbacks on failure[cite: 112, 288, 379].
* [cite_start]**Human-in-the-loop:** The system acts as a "Copilot" or assistant; the human user always makes the final call[cite: 23, 60, 251].

---