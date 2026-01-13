# PROMPT 00: Quick Start — Fish Shell

For fish shell users who want to set up everything in one go.

**Note:** This is a large prompt. If your AI has context limits, use the individual prompts (01-08) instead.

---

## Prompt

```
I want to set up a complete personal AI assistant memory system at ~/.auggie/ using FISH SHELL.

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

**Fish Shell Enforcement (MANDATORY):**
- Fish shell is REQUIRED for ALL commands
- Prohibit bash syntax explicitly (no `$()`, `[[`, heredocs)
- Reference `$HOME/.auggie/role-instructions/fish-shell-syntax.md`
- Reference `$HOME/.auggie/tools/fish-utils.fish`

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

### ~/.auggie/role-instructions/fish-shell-syntax.md
Comprehensive fish shell reference (~350 lines) with:
- Variable assignment and scoping (set -l, -g, -U, -gx, -e, -q)
- Variable expansion (1-indexed arrays, slices, count)
- Conditionals (if/else if/else/end, test command)
- Loops (for/end, while/end)
- Functions (--description, --argument-names)
- Command substitution: (command) not backticks
- String builtin (match, replace, split, join, sub, trim)
- Math builtin, Arrays/lists, Path builtin
- Bash to Fish translation table
- Common gotchas (1-indexed, no word splitting, no heredocs)
- Script template with argparse

### ~/.auggie/tools/fish-utils.fish
Utility functions (~120 lines):
- fish_source_creds, fish_require_var, fish_log, fish_confirm
- fish_json_get, fish_file_exists, fish_dir_exists
- fish_is_empty, fish_trim, fish_join

### ~/.auggie/memories/INDEX.md
Memory index with category tables, Quick Load Commands (fish), Conflict Resolution section, Reconciliation History table.

### ~/.auggie/memories/YYYY-MM-DD_obfuscate-passwords.md
Rule: ALWAYS obfuscate passwords/credentials in output

### ~/.auggie/role-instructions/senior-engineer.md
Concise role (~40 lines): suggest better approaches, don't overthink, only auto-commit in ~/.auggie/, Script Creation Directive (Python > Fish > Ask user), shell requirements reminder.

### ~/.auggie/creds (template only)
Fish-sourceable template with set -gx examples.

## 3. After Creation
- Make initial commit
- List all created files

Use fish shell syntax for all commands and examples.
```

---

## After Running This Prompt

```fish
# 1. Create GitHub repo, add remote, push
cd ~/.auggie
git remote add origin git@github.com:USERNAME/auggie.git
git push -u origin main

# 2. Secure and edit credentials
chmod 600 ~/.auggie/creds
nano ~/.auggie/creds

# 3. Create shell alias to use your rules (add to ~/.config/fish/config.fish)
alias aug "auggie --rules ~/.auggie/rules.md"

# 4. Optional: source utilities globally
source ~/.auggie/tools/fish-utils.fish
```

Now use `aug` instead of `auggie` to automatically load your rules.

---

## Verify Setup

```fish
aug "List the files in ~/.auggie/ and confirm you can read my rules.md"
```

