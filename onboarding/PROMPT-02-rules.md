# PROMPT 02: Create Core Rules

Copy and paste this prompt to create the rules.md file.

---

## Prompt

```
Create ~/.auggie/rules.md with the following structure. This file defines core rules for all AI agent interactions. Keep it concise (~100 lines).

## Section 1: Fish Shell Enforcement (MANDATORY)

Start with a bold header and these exact points:
- Fish shell is REQUIRED for ALL shell commands - no exceptions
- Bash syntax is STRICTLY PROHIBITED: no `$()` for command substitution, no `[[` conditionals, no heredocs `<<EOF`
- Use fish syntax: `()` for command substitution, `test` or `[` for conditionals, `printf` or `echo` for multi-line content
- Reference `$HOME/.auggie/role-instructions/fish-shell-syntax.md` for detailed syntax
- Reference `$HOME/.auggie/tools/fish-utils.fish` for utility functions

## Section 2: Directory Conventions

Add these behavioral rules:
- Don't use `view` or other AI tools for reading files in ~/.auggie/ - use cat/grep/find instead
- Import previous memories from ~/.auggie/memories/
- Check existing tools in ~/.auggie/tools/ before creating new ones
- Import role-based instructions from ~/.auggie/role-instructions/*.md
- If a sub-agent role doesn't have an instruction file, create one
- When told to "remember" something, follow the Memory Creation Protocol
- After any change to ~/.auggie/, commit and push to GitHub

## Section 3: Memory Creation Protocol

4-step process with fish shell examples:

**Step 1: Search for Related Memories**
```fish
grep -l "<keywords>" ~/.auggie/memories/*.md
```
Identify ALL existing memory files on the same topic.

**Step 2: Check for Conflicts**
Look for: contradictory instructions, outdated info, duplicate info that should merge.

**Step 3: Handle Based on Findings**
- No related memories: Create `~/.auggie/memories/YYYY-MM-DD_<name>.md`, update INDEX.md
- Related but no conflict: Append or create new, update INDEX.md
- Conflict detected: ASK USER which is correct, then reconcile

**Step 4: Commit and Sync**
```fish
cd ~/.auggie && git add memories/ && git commit -m "Memory: <brief description>" && git push
```

## Section 4: Sub-Agent Rule Propagation

Explain that sub-agents do NOT inherit rules. When spawning a sub-agent, ALWAYS include these 7 requirements:
1. Fish shell requirement with reference to fish-shell-syntax.md
2. Role-specific instructions reference
3. Credential handling: source from ~/.auggie/creds
4. Git sync requirement
5. Memory access: check INDEX.md, then cat applicable files
6. Memory conflict detection: STOP and ask user if conflicts found
7. Memory creation: follow the Memory Creation Protocol

Include this example sub-agent instruction pattern:
```
<task description>

Requirements:
- Use fish shell syntax for all commands (no bash). Reference: ~/.auggie/role-instructions/fish-shell-syntax.md
- Read relevant memories: Check ~/.auggie/memories/INDEX.md, then cat applicable memory files
- If memories conflict: STOP and ask the user which is correct before proceeding
- When creating memories: Follow Memory Creation Protocol in ~/.auggie/rules.md (search → check conflicts → reconcile)
- Source credentials: source ~/.auggie/creds
- After changes to ~/.auggie/, run: git add, commit, and push
```

Commit the file after creation.
```

---

## Expected Result

- ~/.auggie/rules.md created with all sections
- File committed to git

