# Augroboros Sync Agent Prompt

**Purpose:** Update the prompts in this repository (augroboros) to match the current patterns, structures, and best practices established in the reference repository (\`~/.auggie\`).

---

## Critical Security Rules

> **These rules are NON-NEGOTIABLE:**

### 1. NEVER Commit Sensitive Information

**DO NOT include in any prompt files:**
- Usernames, email addresses, or real names
- API tokens, passwords, or credentials
- Internal hostnames, IP addresses, or URLs
- Project-specific names (Jira project keys, team names)
- Company-specific configuration values
- Any content from \`~/.auggie/creds\`
- Any content from \`~/.auggie/memories/\` that contains identifiable information

**ALWAYS use placeholders:**
- \`YOUR_USERNAME\`, \`YOUR_EMAIL\`, \`YOUR_NAME\`
- \`YOUR_JIRA_TOKEN\`, \`YOUR_GITHUB_TOKEN\`, \`YOUR_API_KEY\`
- \`YOUR_DOMAIN\`, \`YOUR_COMPANY\`, \`YOUR_PROJECT\`
- \`example.com\`, \`api.example.com\`
- \`ED\`, \`EDR\`, \`PROJECT_KEY\` → generic placeholders

### 2. Extract Patterns, Not Data

The goal is to capture **how** things are done, not **what** specific data exists.

| Extract This | Not This |
|--------------|----------|
| Memory file naming convention: \`YYYY-MM-DD_<topic>.md\` | The actual memory filenames |
| INDEX.md category structure | The specific entries in INDEX.md |
| Role instruction file format | Internal URLs/credentials within role files |
| Tool utility patterns | Credentials embedded in tool scripts |
| Error logging patterns | Actual error messages with internal URLs |

---

## Reference Repository

The source of truth is: \`~/.auggie/\`

Key files to analyze for patterns:
- \`~/.auggie/rules.md\` - Core agent rules and protocols
- \`~/.auggie/role-instructions/*.md\` - Role-specific instructions
- \`~/.auggie/tools/*.fish\` - Utility scripts and functions
- \`~/.auggie/memories/INDEX.md\` - Memory index structure (pattern only)
- \`~/.auggie/.gitignore\` - Files to exclude

---

## Sync Tasks

### Task 1: Compare and Update rules.md Patterns (PROMPT-02)

1. Read \`~/.auggie/rules.md\` to identify current patterns
2. Compare with \`onboarding/PROMPT-02-rules.md\`
3. Update PROMPT-02 to generate rules.md that includes:
   - Any new mandatory sections (e.g., error logging, tool restrictions)
   - Updated sub-agent propagation rules
   - New protocols or conventions

**Key additions to look for:**
- Error logging requirements
- Tool restriction policies (\`view\`, \`str-replace-editor\` forbidden)
- Pre-work log checks
- New sub-agent instruction items

### Task 2: Update Fish Utilities Prompt (PROMPT-04)

1. Read \`~/.auggie/tools/fish-utils.fish\` for current functions
2. Compare with \`onboarding/PROMPT-04-fish-utils.md\`
3. Update PROMPT-04 to include:
   - New utility functions (e.g., error logging, credential scrubbing)
   - Updated function signatures
   - New patterns for common operations

**Strip from the source:**
- Hardcoded paths to internal systems
- Credentials or tokens
- Company-specific hostnames

### Task 3: Update Role Instructions Prompt (PROMPT-06)

1. List all role instruction files in \`~/.auggie/role-instructions/\`
2. **Genericize ALL roles** — no roles should be excluded
3. Update PROMPT-06 to create genericized versions of all roles

**Genericization process for each role:**
- Remove internal hostnames, URLs, and IP addresses → use \`example.com\`, \`api.example.com\`
- Remove company-specific project keys → use \`PROJECT_KEY\`, \`YOUR_PROJECT\`
- Remove specific tool instance URLs (e.g., Jira, NetBox URLs) → use placeholders
- Remove team names and org-specific terminology → use generic equivalents
- Keep the structure, patterns, and best practices intact
- Keep tool-specific knowledge (API patterns, CLI usage, workflow patterns)

**Roles to genericize (include ALL):**

| Role File | Genericization Notes |
|-----------|---------------------|
| \`senior-engineer.md\` | Keep best practices, script policies |
| \`fish-shell-syntax.md\` | Shell reference — likely already generic |
| \`tool-restrictions.md\` | Keep tool restriction patterns |
| \`error-handling.md\` | Logging patterns — genericize any URLs |
| \`netbox-engineer.md\` | Replace NetBox URLs, keep API patterns |
| \`k8s-engineer.md\` | Replace cluster names/contexts, keep K8s patterns |
| \`scrum-master.md\` | Replace Jira URLs/project keys, keep workflow patterns |
| \`security-engineer.md\` | Keep security patterns and practices |
| \`technical-writer.md\` | Keep documentation patterns |
| \`ai-prompt-engineer.md\` | Keep prompt engineering patterns |

**Example genericization:**

Before (company-specific):
\`\`\`markdown
NETBOX_URL="https://netbox.internal.company.com"
Default project: ED (Engineering Delivery)
Jump host: ssh admin@jumpbox.corp.example.com
\`\`\`

After (genericized):
\`\`\`markdown
NETBOX_URL="https://netbox.example.com"  # Replace with your NetBox instance
Default project: YOUR_PROJECT_KEY  # Replace with your project
Jump host: ssh YOUR_USER@YOUR_JUMPHOST  # Replace with your jump host
\`\`\`

**Key principle:** Users should be able to search-and-replace placeholders with their own values.


### Task 4: Check for New Structural Patterns

1. Compare directory structure of \`~/.auggie/\` with what prompts generate
2. Identify any new directories or files (e.g., \`logs/\`)
3. Update relevant prompts to include new structure

**Check for:**
- New directories (config/, logs/, standups/)
- New required files
- Updated .gitignore patterns

### Task 5: Update Quick Start Prompts (PROMPT-00)

1. Review \`PROMPT-00-quickstart-fish.md\` and \`PROMPT-00-quickstart-bash.md\`
2. Ensure they reflect ALL updates from tasks 1-4
3. Add any new post-setup instructions

### Task 6: Update README Files

1. Update \`README.md\` to reflect any new features/patterns
2. Update \`onboarding/README.md\` with correct file lists and descriptions
3. Ensure all referenced files actually exist

---

## Validation Checklist

Before committing changes, verify:

- [ ] **No PII**: grep for usernames, emails, internal hostnames
- [ ] **No credentials**: grep for \`TOKEN\`, \`PASSWORD\`, \`SECRET\`, \`KEY=\`
- [ ] **No internal URLs**: grep for \`.internal\`, company domain patterns
- [ ] **Placeholders used**: All examples use \`YOUR_*\` or \`example.com\`
- [ ] **Structure matches**: Prompts generate same directory structure as reference
- [ ] **Rules match**: PROMPT-02 produces rules.md with same sections as reference
- [ ] **Utils match**: PROMPT-04 produces utilities with same functions as reference

### Validation Commands

\`\`\`fish
# Check for potential PII/credentials in changed files
git diff --name-only | xargs grep -i -E "(token|password|secret|username|email)" 2>/dev/null

# Check for internal URLs
git diff --name-only | xargs grep -i -E "\.(internal|corp|company)" 2>/dev/null

# Ensure placeholders are used
git diff --name-only | xargs grep -E "YOUR_|EXAMPLE_|example\.com" 2>/dev/null
\`\`\`

---

## Diff Strategy

For each file, follow this workflow:

1. **Read reference** - Load the ~/.auggie version
2. **Read current prompt** - Load the onboarding/ version  
3. **Identify gaps** - What does reference have that prompt doesn't generate?
4. **Abstract gaps** - Remove specific data, keep patterns
5. **Update prompt** - Add abstracted patterns to prompt
6. **Validate** - Ensure no PII leaked into prompt

---

## Output Format

After completing sync, provide:

1. **Summary of changes** - What was added/updated in each prompt
2. **Validation report** - Confirmation that no PII was included
3. **Manual review items** - Anything requiring human judgment

---

## Requirements

- Use fish shell syntax for all commands (no bash)
- Reference: \`~/.auggie/role-instructions/fish-shell-syntax.md\`
- Read files with \`cat\`, \`grep\`, \`head\`, \`tail\` - NOT with \`view\` tool
- Modify files with \`sed\`, \`awk\`, \`tee\`, \`printf\` - NOT with \`str-replace-editor\` tool
- After changes, commit and push to GitHub

---

## Example: Extracting a Pattern

**Source (from ~/.auggie/role-instructions/error-handling.md):**
\`\`\`markdown
| Pattern | Example | Logged As |
|---------|---------|-----------|
| Bearer tokens | \`Authorization: Bearer abc123...\` | \`Authorization: Bearer [REDACTED]\` |
\`\`\`

**Abstracted (for augroboros prompt):**
\`\`\`markdown
Include a table of credential patterns that should be auto-redacted:
- Bearer tokens
- Basic auth headers
- curl -u credentials
- URL credentials
- JWT tokens
- API keys in query params
- JSON password fields
- Environment variables with sensitive names
\`\`\`

The pattern is preserved. The specific internal token is not.

---

## Remember

You are updating prompts that OTHER USERS will run to create THEIR OWN \`~/.auggie/\` directories.

- These users have different usernames, different APIs, different companies
- The prompts must work generically for anyone
- All configuration should use placeholders they can fill in
- The ~/.auggie/ reference is the gold standard for STRUCTURE and PATTERNS
- The ~/.auggie/ reference is NOT to be copied for CONTENT
