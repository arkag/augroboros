# PROMPT 00: Quick Start — Bash Shell

For bash shell users who want to set up everything in one go.

**Note:** This is a large prompt. If your AI has context limits, use the individual prompts (01-08) instead.

---

## Prompt

```
I want to set up a complete personal AI assistant memory system at ~/.auggie/ using BASH SHELL.

Please create the following structure and files:

## 1. Directory Structure
~/.auggie/
├── memories/
├── role-instructions/
├── tools/
├── config/
└── standups/

Initialize as a git repository with .gitignore (exclude .DS_Store, *.pyc, __pycache__, .env, *.log, creds).

## 2. Core Files to Create

### ~/.auggie/rules.md
Create a concise rules file (~100 lines) with these sections:

**Bash Shell Enforcement (MANDATORY):**
- Bash shell is REQUIRED for ALL commands
- Prohibit fish syntax explicitly (no `set` for variables, no `; and`/`; or` chaining)
- Reference `$HOME/.auggie/role-instructions/bash-shell-syntax.md`
- Reference `$HOME/.auggie/tools/bash-utils.sh`

**Directory conventions:**
- Don't use AI tools (view, codebase-retrieval) for ~/.auggie/ - use cat/grep instead
- Import memories from ~/.auggie/memories/
- Check existing tools before creating new ones
- Import role instructions from ~/.auggie/role-instructions/

**Memory Creation Protocol (4 steps):**
1. Search: `grep -l "<keywords>" ~/.auggie/memories/*.md`
2. Check for conflicts (contradictions, outdated info, duplicates)
3. Handle: create new, append, or ask user if conflict detected
4. Commit and sync: `git add && git commit && git push`

**Sub-Agent Rule Propagation (7 rules):**
Sub-agents don't inherit rules. Include: shell requirement, role-specific instructions, credential handling, git sync, memory access, conflict detection, memory creation protocol.

Include an example sub-agent instruction pattern.

### ~/.auggie/role-instructions/bash-shell-syntax.md
Comprehensive bash shell reference (~300 lines) with:
- Variable assignment: VAR="value" (no spaces around =)
- Exporting: export VAR="value"
- Command substitution: $(command) or backticks
- Conditionals: if [[ ]]; then; elif; else; fi
- Test operators: -f, -d, -e, -z, -n, ==, !=, -eq, -lt, -gt
- Loops: for x in list; do; done and while; do; done
- Functions: name() { } syntax
- Arrays: arr=(a b c), ${arr[0]} (0-indexed), ${#arr[@]}
- String manipulation: ${var#pattern}, ${var%pattern}, ${var/find/replace}
- Here-documents: <<EOF
- Arithmetic: $((expr)) and let
- Common patterns and best practices
- Script template with getopts

### ~/.auggie/tools/bash-utils.sh
Utility functions (~120 lines):
- bash_source_creds, bash_require_var, bash_log, bash_confirm
- bash_json_get, bash_file_exists, bash_dir_exists
- bash_is_empty, bash_trim, bash_join

### ~/.auggie/memories/INDEX.md
Memory index with category tables, Quick Load Commands (bash), Conflict Resolution section, Reconciliation History table.

### ~/.auggie/memories/YYYY-MM-DD_obfuscate-passwords.md
Rule: ALWAYS obfuscate passwords/credentials in output

### ~/.auggie/role-instructions/senior-engineer.md
Concise role (~40 lines): suggest better approaches, don't overthink, only auto-commit in ~/.auggie/, Script Creation Directive (Python > Bash > Ask user), shell requirements reminder.

### ~/.auggie/creds (template only)
Bash-sourceable template with export VAR="value" examples.

## 3. After Creation
- Make initial commit
- List all created files

Use bash shell syntax for all commands and examples.
```

---

## After Running This Prompt

```bash
# 1. Create GitHub repo, add remote, push
cd ~/.auggie
git remote add origin git@github.com:USERNAME/auggie.git
git push -u origin main

# 2. Secure and edit credentials
chmod 600 ~/.auggie/creds
nano ~/.auggie/creds

# 3. Create shell alias to use your rules (add to ~/.bashrc or ~/.bash_profile)
alias aug="auggie --rules ~/.auggie/rules.md"

# 4. Optional: source utilities globally (add to ~/.bashrc)
source ~/.auggie/tools/bash-utils.sh

# 5. Reload shell
source ~/.bashrc
```

Now use `aug` instead of `auggie` to automatically load your rules.

---

## Verify Setup

```bash
aug "List the files in ~/.auggie/ and confirm you can read my rules.md"
```

