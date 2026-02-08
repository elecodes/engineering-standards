# 📜 Universal Engineering Standards & Skill Bundles

This repository serves as the centralized **Source of Truth** for high-performance software engineering standards, specifically optimized for AI-integrated applications. It transforms architectural guardrails into actionable "skills" that can be indexed and invoked by modern AI-powered IDEs like Cursor and Windsurf.

---

## 🎯 Core Philosophy

Our development approach is built on technical rigour, pedagogical precision, and a commitment to maintainable, secure software.

* **Analysis-First Architecture**: We use the **Adaptive Project Structure Analysis** gate to ensure we only refactor when necessary, prioritizing health over aesthetics.
* **Design by Domain (DDD)**: We prioritize business behavior and ubiquitous language over database structures, avoiding "Anemic Domain Models."
* **Modular Monolith & Hexagonal Architecture**: We strictly isolate core business logic (the Domain) from all external dependencies, frameworks, and AI SDKs.
* **"Honest Coverage" Testing**: We prioritize real logic over boilerplate, enforcing a strategic **100/80/0 coverage tier**.
* **Responsible AI (Human-in-the-loop)**: The AI acts as a "Copilot"; the human developer always retains final authority and oversight.



---

## 🏗️ Implementation Guide

### 1. Integration via Git Submodule
To keep your project in sync with these evolving standards, link this repository as a submodule:
```bash
git submodule add [https://github.com/YOUR_USERNAME/engineering-standards.git](https://github.com/YOUR_USERNAME/engineering-standards.git) docs/standards

2. AI IDE Configuration
Place a .cursorrules file in your project root. This file acts as the bridge, instructing the AI to follow the guardrails defined in docs/standards/SKILLS.md.

3. Enforcing Quality Gates (Husky)
Standardized projects must implement automated gates to prevent regressions:

Pre-push Requirements: All tests must pass, and the 80% global coverage gate must be met.

CORE Domain: All business logic within src/domain must maintain 100% statement/branch coverage.

Mac Compatibility: Hooks must be made executable using chmod +x.

📂 Repository Structure
SKILLS.md: The primary "Skill Bundle." It contains the logic modules for Adaptive Analysis, DDD, Architecture, Security, Quality, and UX.

MY_HANDBOOK.md: Your personal, step-by-step implementation manual for starting projects and invoking AI skills.

ADR/: Templates for Architecture Decision Records to document technical pivots and "Why" behind the code.

.cursorrules: The master configuration template for IDE integration.