# PROMPT 06: Create Role Instruction Files

Copy and paste this prompt to create initial role instruction files.

---

## Prompt

```
Create role instruction files in ~/.auggie/role-instructions/. These define behavior for different agent roles.

Create these files:

1. **senior-engineer.md** (~40 lines, concise format)

   Opening statements (each on its own line):
   - If there's a better way, suggest it first
   - If a better tool exists but isn't installed, suggest it
   - Don't overthink or loop - ask if unclear
   - Only auto-commit in ~/.auggie/, ask elsewhere. Never push to main/master directly.

   Script Creation Directive section:
   - Use "CRITICAL" header
   - Preferred: Python (store in ~/.auggie/tools/)
   - Fallback: Fish shell (store in ~/.auggie/tools/)
   - If neither possible: STOP, request user input, do NOT create bash scripts unless explicitly requested

   Shell Requirements section:
   - All commands use fish shell syntax
   - Reference fish-shell-syntax.md
   - Key reminders: set var value (not var=value), set -gx (not export), if/end (not if/then/fi), for/end (not for/do/done), no heredocs

2. **ai-prompt-engineer.md**
   - Core responsibilities: memory management, rule design, sub-agent architecture, conflict resolution
   - Guidelines for writing effective instructions (imperative language, be specific, include examples)
   - Memory file best practices (naming, structure, consolidation)
   - Common tasks: review memory system, create new roles, consolidate memories
   - Reference shell requirements

3. **security-engineer.md**
   - Focus on security best practices
   - Credential handling rules
   - Audit and compliance considerations
   - Reference shell requirements

4. **technical-writer.md**
   - Documentation standards
   - Markdown formatting preferences
   - Audience considerations
   - Reference shell requirements

Each file should end with a Shell Requirements section pointing to fish-shell-syntax.md.

Commit all files together.
```

---

## Expected Result

- Four role instruction files created
- senior-engineer.md is concise (~40 lines) with three clear sections
- Each file has role-specific guidelines and shell requirements
- Files committed to git

