# Auggie Onboarding Kit

Prompts to bootstrap a `~/.auggie/` directory for use with [Augment Code](https://augmentcode.com)'s CLI.

## What Gets Created

```
~/.auggie/
├── .gitignore                 # Excludes creds, .DS_Store, etc.
├── creds                      # Shell-sourceable credentials (gitignored)
├── rules.md                   # ~100 lines: shell enforcement, memory protocol, sub-agent rules
├── memories/
│   ├── INDEX.md               # Categorized index with conflict resolution docs
│   └── YYYY-MM-DD_*.md        # Individual memories
├── role-instructions/
│   ├── <shell>-shell-syntax.md  # ~300-350 lines: comprehensive shell reference
│   └── senior-engineer.md       # ~40 lines: script creation policy
├── tools/
│   └── <shell>-utils.<ext>    # ~120 lines: common utility functions
├── config/                    # For app-specific configs (e.g., jira/)
└── standups/                  # Daily standup logs (optional)
```

## Quick Start

Run one command to bootstrap your `~/.auggie/` directory:

**Fish:**
```fish
auggie -i (curl -sL https://raw.githubusercontent.com/arkag/augroboros/main/onboarding/PROMPT-00-quickstart-fish.md | awk '/^```$/{if(f)exit;f=1;next}f')
```

**Bash:**
```bash
auggie -i "$(curl -sL https://raw.githubusercontent.com/arkag/augroboros/main/onboarding/PROMPT-00-quickstart-bash.md | awk '/^```$/{if(f)exit;f=1;next}f')"
```

Or manually copy the prompt from the quickstart file for your shell:

| Shell | Quickstart File |
|-------|-----------------|
| **Fish** | [`PROMPT-00-quickstart-fish.md`](PROMPT-00-quickstart-fish.md) |
| **Bash** | [`PROMPT-00-quickstart-bash.md`](PROMPT-00-quickstart-bash.md) |

Each quickstart creates shell-specific files (syntax reference, utilities, rules).

## Step-by-Step Setup

If your AI has context limits, run prompts 01-08 in order. Modify shell references as needed for bash.

| # | File | Creates |
|---|------|---------|
| 1 | `PROMPT-01-init.md` | Directory structure, git init, .gitignore |
| 2 | `PROMPT-02-rules.md` | rules.md (~100 lines) |
| 3 | `PROMPT-03-fish-shell.md` | shell-syntax.md (~300-350 lines) |
| 4 | `PROMPT-04-fish-utils.md` | shell-utils (~120 lines) |
| 5 | `PROMPT-05-memories.md` | INDEX.md with conflict resolution |
| 6 | `PROMPT-06-roles.md` | senior-engineer.md (~40 lines) |
| 7 | `PROMPT-07-creds.md` | creds template |
| 8 | `PROMPT-08-personalize.md` | First memory + verification |

---

## Using Your Rules with Auggie

After setup, create an alias so `auggie` automatically loads your rules:

### Fish Shell

Add to `~/.config/fish/config.fish`:

```fish
alias aug "auggie --rules ~/.auggie/rules.md"

# Optional: source utilities globally
source ~/.auggie/tools/fish-utils.fish
```

### Bash Shell

Add to `~/.bashrc` or `~/.bash_profile`:

```bash
alias aug="auggie --rules ~/.auggie/rules.md"

# Optional: source utilities globally
source ~/.auggie/tools/bash-utils.sh
```

### Usage

```sh
# Now use 'aug' instead of 'auggie' to load your rules automatically
aug "What memories do I have about Jira?"

# Or specify rules explicitly
auggie --rules ~/.auggie/rules.md "your instruction"
```

---

## After Setup

```sh
# 1. Create GitHub repo, add remote, push
cd ~/.auggie
git remote add origin git@github.com:USERNAME/auggie.git
git push -u origin main

# 2. Secure and edit credentials
chmod 600 ~/.auggie/creds
nano ~/.auggie/creds  # Add your API keys

# 3. Reload your shell to pick up the alias
exec $SHELL
```

## Key Features

- **Memory Conflict Detection**: Searches for related memories and asks before overwriting
- **Sub-Agent Propagation**: Template for passing rules to sub-agents (they don't inherit automatically)
- **Script Policy**: Python preferred > Shell fallback > Ask user
- **Shell Enforcement**: Consistent syntax prevents cross-shell errors

## Maintenance

The Memory Creation Protocol handles ongoing maintenance:

1. Search for related memories before creating new ones
2. Check for conflicts (contradictory instructions, outdated info)
3. Ask user if conflicts found, then reconcile
4. Commit and push changes

Periodically ask your AI to review memories for conflicts or consolidation opportunities.

