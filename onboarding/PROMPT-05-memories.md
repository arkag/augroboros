# PROMPT 05: Set Up Memory System

Copy and paste this prompt to create the memory system.

---

## Prompt

```
Set up the memory system in ~/.auggie/memories/.

Create ~/.auggie/memories/INDEX.md with the following structure:

**Header**:
- Purpose: Quick reference for agents/sub-agents to find memories by category
- Usage: Scan relevant categories and read applicable files before starting tasks
- Last Reconciled: YYYY-MM-DD

**Category Tables** (each as markdown table with File | Description columns):

1. **Infrastructure & Networking** - for configs, network settings, switches, etc.
2. **Security & Credentials** - Start with obfuscate-passwords.md
3. **Scripts & Tools** - for script policies, shell patterns
4. **Sprints & Standups** - for standup rules, sprint info
5. **Projects** - for project-specific memories
6. **User Preferences** - for personal preferences

**Quick Load Commands** section with fish examples:

\`\`\`fish
# Load all memories in a category
cat ~/.auggie/memories/*network*.md

# Search for keyword
grep -l "keyword" ~/.auggie/memories/*.md

# List all memories
ls -la ~/.auggie/memories/
\`\`\`

**Conflict Resolution** section with:

- **What is a Memory Conflict?** - Contradictory instructions about same topic
- **Detection: When to Check** - List of 5 things to scan for:
  1. Version differences
  2. Contradictory instructions
  3. Different endpoints
  4. Conflicting auth
  5. Outdated dates
- **What to Do When Conflicts Found**: STOP and ask user template
- **Do NOT**: Guess, proceed with either, silently pick newer
- **Reconciliation Process**: Create consolidated file, delete conflicting files, update INDEX.md, commit
- **Naming convention**: YYYY-MM-DD_<topic>-consolidated.md
- **Reconciliation History** table: Date | Action | Files Affected

Also create a starter memory file:
~/.auggie/memories/YYYY-MM-DD_obfuscate-passwords.md with the rule:
"ALWAYS obfuscate passwords, credentials, and sensitive information in output"

Update INDEX.md to include this file in Security & Credentials.

Commit both files.
```

---

## Expected Result

- ~/.auggie/memories/INDEX.md with full structure
- Starter memory file for password obfuscation
- Files committed to git

