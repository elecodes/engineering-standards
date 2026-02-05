# 📜 Universal Engineering Standards & Skill Bundles

This repository serves as the centralized **Source of Truth** for high-performance software engineering standards, specifically optimized for AI-integrated applications. It transforms architectural guardrails into actionable "skills" that can be indexed and invoked by modern AI-powered IDEs.

---

## 🎯 Core Philosophy

Our development approach is built on technical rigour, pedagogical precision, and a commitment to maintainable, secure software.

* [cite_start]**Modular Monolith & Hexagonal Architecture**: We strictly isolate core business logic (the Domain) from all external dependencies, frameworks, and AI SDKs[cite: 12, 13, 47].
* [cite_start]**"Honest Coverage" Testing**: We prioritize real logic over boilerplate inflation, enforcing a strategic 100/80/0 coverage tier[cite: 137, 138, 173].
* [cite_start]**Responsible AI (Human-in-the-loop)**: The system acts as a "Copilot" to assist decision-making; the human user always retains the final authority[cite: 23, 60, 251].

---

## 🏗️ Implementation Guide

### 1. Integration via Git Submodule
To keep your project in sync with these evolving standards, add this repository as a submodule:
```bash
git submodule add [https://github.com/YOUR_USERNAME/engineering-standards.git](https://github.com/YOUR_USERNAME/engineering-standards.git) docs/standards

2. AI IDE Configuration
Add a .cursorrules file to your project root and point it to docs/standards/SKILLS.md to ensure the AI assistant follows these guardrails automatically.

3. Enforcing Quality Gates (Husky)
Standardized projects must implement Husky to block code that fails our quality thresholds.


Pre-push Requirements: All tests must pass, and the 80% global coverage gate must be met.


CORE Domain: Critical functions within src/domain must maintain 100% statement/branch coverage.

📂 Repository Structure
SKILLS.md: The primary "Skill Bundle" for AI IDE instructions.

MY_HANDBOOK.md: Private implementation guide for developers.


ADR/: (Optional) Templates for Architecture Decision Records to document technical pivots.