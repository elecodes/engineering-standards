# 📜 Universal Engineering Standards & Skill Bundles

This repository serves as the centralized **Source of Truth** for high-performance software engineering standards, specifically optimized for AI-integrated applications. It transforms architectural guardrails into actionable "skills" that can be indexed and invoked by modern AI-powered IDEs (such as Cursor or Windsurf).

---

## 🎯 Core Philosophy

[cite_start]Our development approach is built on technical rigour, pedagogical precision, and a commitment to maintainable, secure software[cite: 6, 29].

* [cite_start]**Modular Monolith & Hexagonal Architecture**: We strictly isolate core business logic (the Domain) from all external dependencies, frameworks, and AI SDKs[cite: 12, 13, 47].
* [cite_start]**"Honest Coverage" Testing**: We prioritize real logic over boilerplate inflation, enforcing a strategic 100/80/0 coverage tier[cite: 137, 138, 173].
* [cite_start]**Responsible AI (Human-in-the-loop)**: The system acts as a "Copilot" to assist decision-making; the human user always retains the final authority[cite: 23, 60, 251].

---

## 🛠 How to Use with AI IDEs

To provide your AI assistant with the necessary context and guardrails, point it to the `SKILLS.md` file in this repository.

### 1. IDE Configuration
Add this repository URL or the local path of the `SKILLS.md` file to your "Rules for AI" or "Project Settings."

### 2. Invoking Skills
Once indexed, you can invoke specific modules during your chat sessions:
* *"Review this directory using the **ArchitectureDesign** skill."*
* *"Generate unit tests for this domain entity following the **QualityTesting** skill."*
* *"Audit this API endpoint using the **SecurityDevSecOps** skill."*

---

## 🏗 Implementation Guide

### 1. Integration via Git Submodule (Recommended)
To keep your project in sync with these evolving standards, add this repository as a submodule:
```bash
git submodule add [https://github.com/YOUR_USERNAME/engineering-standards.git](https://github.com/YOUR_USERNAME/engineering-standards.git) docs/standards

2. Enforcing Quality Gates (Husky)
Standardized projects must implement Husky to block code that fails our quality thresholds before it reaches the repository.


Pre-push Requirements: All tests must pass, and the 80% global coverage gate must be met.


CORE Domain: Critical functions within src/domain must maintain 100% statement/branch coverage.

3. Security Standards
Every project must adhere to the following by default:


Strict Input Validation: Use Zod or Valibot for every piece of external data.


AI Defense: Implement prompt injection defenses and ensure data privacy (no training on user data).


Secure Headers: Use Helmet.js for all web-facing interfaces.

📂 Repository Structure
SKILLS.md: The primary "Skill Bundle" for AI IDE instructions.

README.md: This manual.


ADR/: (Optional) Templates for Architecture Decision Records to document technical pivots.

TEMPLATES/: Boilerplate for Husky hooks and CI/CD pipelines.




### Why this structure works
* **Traceability**: It explicitly references the "Honest Coverage" system[cite: 138, 173].
* **Resilience**: It mandates the use of **Husky** to ensure that defective code never enters your repository[cite: 198, 203].
* **Clarity**: It defines the system as an assistant, ensuring the developer (you) remains the decision-maker[cite: 60].

**Would you like me to create a `.cursorrules` file content to make sure your IDE automatically reads these standards every time you start a chat?**