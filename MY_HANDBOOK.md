# 📓 My Personal Engineering Handbook
*Simple, clear steps for high-standard implementation.*

## 🚀 Setting Up a New Project (The Workflow)
1.  **Repo Link**: Link the "Brain" by running `git submodule add [REPO_URL] docs/standards`.
2.  **AI Config**: Place `.cursorrules` in your project root. This forces the AI to read your skills before writing code.
3.  **Husky Setup**: 
    * Initialize: `npx husky init`.
    * Create `.husky/pre-push` (manually in v9+).
    * Apply `chmod +x .husky/pre-push` to make it work on Mac.
* ## 🧠 How to Invoke the "Husky Skill" (IDE Instructions)

When you want the AI to set up your repository protections, use the following prompt:

> **"Invoke the QualityTesting skill to set up the repository guards for this project."**

### What the AI will do:
1. **Initialize Husky**: It will run `npx husky init` to create the `.husky` directory.
2. **Create the Guard**: It will create the `.husky/pre-push` file.
3. **Inject the Logic**: It will add the script to run `npm run test:coverage` and check for the 80% threshold.
4. **macOS Compliance**: It will remind you (or attempt) to run `chmod +x .husky/pre-push` to ensure the script can run on your MacBook Air.

### Manual Verification
If you need to verify it manually, ensure `.husky/pre-push` contains:
```bash
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

echo "🛡️ Checking Honest Coverage (80% Global)..."
npm run test:coverage -- --coverage.thresholds.global.statements=80

## 📊 The "Honest Coverage" Cheat Sheet
Focus testing resources where they matter most[cite: 174, 175]:
* **CORE (100% Mandatory)**: Business rules and AI evaluators in `src/domain`. Zero tolerance for logic errors [cite: 139, 176-179].
* **GLOBAL (80% Target)**: Orchestration, UI, and application services [cite: 140, 180-184].
* **INFRA (0% Excluded)**: Purely static or type-checking code. Exclude from metrics to avoid "noise" [cite: 141, 185-187].

## 🧠 Skill Invocations (Talk to the AI)
Use these phrases to trigger specific architectural guardrails:
* **"Apply ArchitectureDesign"**: Decouples logic from the DB and keeps the Domain "pure"[cite: 13, 318].
* **"Apply SecurityDevSecOps"**: Triggers Zod validation and OWASP LLM defenses[cite: 52, 56, 271].
* **"Apply ResilientLogic"**: Adds retries, timeouts, and circuit breakers for AI calls[cite: 17, 114].
* **"Apply UniversalUX"**: Ensures WCAG 2.1 AA accessibility and <100ms feedback[cite: 373, 379].

## 🛠 Architectural Laws
* **Traceability**: Every AI response must include source excerpts and a decision log[cite: 20, 59, 154].
* **Idempotency**: Processing the same file twice must yield the same result[cite: 19].
* **Statelessness**: Services must be stateless for horizontal scaling[cite: 18].
Final Steps to Complete your Repo: