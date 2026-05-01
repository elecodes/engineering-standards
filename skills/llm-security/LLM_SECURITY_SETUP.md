# 📋 How to Add LLM Security Skill to Your Engineering Standards Repo

## What's Been Done

I've added the **LLM Security & AI Agent Hardening** skill to your engineering standards repository:

### Files Modified:
1. **SKILLS.md** – Added `LLMSecurityAI` skill definition (12 bullet points covering all key domains)
2. **README.md** – Updated repository structure to reference the new skill and skills/ directory
3. **skills/llm-security.md** (new) – Detailed guide with implementation patterns, code examples, and checklists

## Structure

```
engineering-standards/
├── README.md (updated)
├── SKILLS.md (updated with LLMSecurityAI skill)
├── skills/
│   └── llm-security.md (new - detailed guide)
├── cursorrules
└── .git/
```

## What Changed in SKILLS.md

Added a new skill section:

```markdown
## 🔐 Skill: LLMSecurityAI
*Secure AI agents and LLM applications against prompt injection, jailbreaks, and tool misuse.*

* **Assume Compromise**: Treat the model as a potential vulnerability...
* **Defense in Depth**: Layer security at input (gateways), tools (contracts)...
* **Tool Contract Enforcement**: Use strongly-typed schemas...
* **Input Gateway**: Detect and block prompt injection patterns...
* **Output Gateway**: Redact PII, detect jailbreaks, sanitize code...
* **RAG Safety**: Validate document sources, re-rank by trustworthiness...
* **Resilience**: Timeouts, retries, circuit breakers, rate limits...
* **Observability**: Log all interactions with metrics...
* **Supply Chain**: Verify model source/checksum...
* **OWASP Top 10 for LLMs**: Core mitigations...
* **Sensitive Action HITL**: Require approval for critical actions...
* **Jailbreak Patterns**: Block common manipulation patterns...
```

## How to Push to GitHub

### Option 1: Push from your local machine

```bash
# 1. Copy the changes from /home/claude/engineering-standards/ to your local repo
cd ~/path/to/your/engineering-standards

# 2. Or if you want to pull from this version:
git clone /home/claude/engineering-standards my-updated-standards
cd my-updated-standards

# 3. Create a new branch (optional but recommended)
git checkout -b feat/add-llm-security

# 4. Review the changes
git status
# Should show:
#   modified:   README.md
#   modified:   SKILLS.md
#   new file:   skills/llm-security.md

# 5. Commit
git commit -m "feat: add LLM Security AI skill

- Add LLMSecurityAI skill to SKILLS.md covering:
  - System architecture and isolation
  - Tool contract enforcement
  - Input/output gateway patterns
  - RAG security
  - OWASP Top 10 for LLMs
  - Observability and metrics
  - Supply chain verification
- Create skills/llm-security.md with detailed implementation guide
- Update README.md to reference new skill"

# 6. Push to GitHub
git push origin feat/add-llm-security

# 7. Create a Pull Request on GitHub if you want review, or:
git checkout main
git merge feat/add-llm-security
git push origin main
```

### Option 2: Drag and drop (if using GitHub web UI)

1. Go to https://github.com/elecodes/engineering-standards
2. Go to **Add file** → **Create new file**
3. Create `skills/llm-security.md` and paste the detailed skill content
4. Go to **Files** → **Edit** on `SKILLS.md` and add the LLMSecurityAI skill section
5. Do the same for `README.md`

---

## How to Use in Your Projects

Once pushed, invoke the skill in your projects:

### In SKILLS.md or project docs:
```
"Review this agent for security using the **LLMSecurityAI** skill."
```

### In .cursorrules:
```
Refer to the LLMSecurityAI skill from the engineering-standards 
for prompt injection defense, tool safety, and output filtering.
```

### In code/PRs:
```
This implements the LLMSecurityAI skill requirements:
- Input gateway for prompt injection detection
- Output gateway for PII redaction
- Tool contracts with rate limits
- Observability logging
```

---

## Next Steps

1. **Push the changes** to GitHub (choose Option 1 or 2 above)
2. **Create a commit/PR** with message about adding LLM Security
3. **Use in projects** by referencing `LLMSecurityAI` skill in:
   - New project `.cursorrules`
   - README for AI agent projects
   - Code reviews for LLM-based features
4. **Customize per project** by:
   - Reading SKILLS.md (quick summary)
   - Reading skills/llm-security.md (detailed guide)
   - Implementing top 3 threats for your specific project
   - Tuning thresholds/policies as needed

---

## File Contents Reference

### SKILLS.md Entry (Brief)
The SKILLS.md entry is a **quick reference** that summarizes the skill in bullet points. Use when you need a quick checklist or to invoke the skill in conversations.

### skills/llm-security.md (Detailed)
The detailed guide includes:
- Core principles
- 10 security domains with code examples
- OWASP Top 10 mapping
- Per-project customization guidance
- Implementation workflows
- Pre-launch and ongoing checklists

---

## Questions?

- **Where's the concise version?** – In SKILLS.md
- **Where's the detailed version?** – In skills/llm-security.md
- **Can I customize it per project?** – Yes! Create a project-specific `PROJECT_SECURITY.md` that references both SKILLS.md and skills/llm-security.md, then customize the top 3 risks for your use case
- **How do I invoke this in Cursor/Windsurf?** – Add to `.cursorrules`: "Use the LLMSecurityAI skill from engineering-standards to secure any AI agent code."

---

Good luck with the push! 🚀
