# Auggie

**A persistent memory and tooling framework for AI coding assistants.**

Auggie gives your AI assistant long-term memory, consistent behavior, and reusable tools—all synced to GitHub so it follows you across sessions and machines.

## What Is This?

When you use an AI coding assistant (like Augment), each conversation starts fresh. Auggie solves this by creating a `~/.auggie/` directory that your AI reads at the start of every session, containing:

- **Memories** — Facts, preferences, and context that persist across conversations
- **Rules** — Behavioral guidelines (shell syntax, git workflows, safety protocols)
- **Role Instructions** — Specialized prompts for different agent personas
- **Tools** — Scripts and utilities the AI can invoke
- **Credentials** — API keys and tokens (gitignored)

## Quick Start

### Option A: All-in-One Setup

Choose your shell and copy the corresponding quickstart prompt into a new conversation:

| Shell | Quickstart |
|-------|------------|
| **Fish** | [`PROMPT-00-quickstart-fish.md`](onboarding/PROMPT-00-quickstart-fish.md) |
| **Bash** | [`PROMPT-00-quickstart-bash.md`](onboarding/PROMPT-00-quickstart-bash.md) |

### Option B: Step-by-Step Setup

Run these prompts in order. The step-by-step prompts default to fish shell—modify references for bash as needed.

| Step | File | What It Does |
|------|------|--------------|
| 1 | [`PROMPT-01-init.md`](onboarding/PROMPT-01-init.md) | Create directory structure, init git repo |
| 2 | [`PROMPT-02-rules.md`](onboarding/PROMPT-02-rules.md) | Core rules and protocols |
| 3 | [`PROMPT-03-fish-shell.md`](onboarding/PROMPT-03-fish-shell.md) | Shell syntax reference |
| 4 | [`PROMPT-04-fish-utils.md`](onboarding/PROMPT-04-fish-utils.md) | Utility functions |
| 5 | [`PROMPT-05-memories.md`](onboarding/PROMPT-05-memories.md) | Memory system with INDEX.md |
| 6 | [`PROMPT-06-roles.md`](onboarding/PROMPT-06-roles.md) | Role instruction files |
| 7 | [`PROMPT-07-creds.md`](onboarding/PROMPT-07-creds.md) | Credentials template |
| 8 | [`PROMPT-08-personalize.md`](onboarding/PROMPT-08-personalize.md) | Your preferences + first memories |

### After Setup

1. Create a GitHub repo (public or private)
2. Add the remote and push:
   ```sh
   cd ~/.auggie
   git remote add origin git@github.com:YOUR_USERNAME/auggie.git
   git push -u origin main
   ```
3. Edit `~/.auggie/creds` with your actual API keys
4. Create a shell alias to load your rules automatically (see below)

## Using Your Rules

After setup, create an alias so auggie automatically loads your rules:

**Fish** — add to `~/.config/fish/config.fish`:
```fish
alias aug "auggie --rules ~/.auggie/rules.md"
```

**Bash** — add to `~/.bashrc`:
```bash
alias aug='auggie --rules ~/.auggie/rules.md'
```

Now use `aug` instead of `auggie`:
```sh
aug "What do you remember about my Jira setup?"
```

## How It Works

### Memory System

Tell your AI to "remember" something and it will:

1. Search existing memories for related content
2. Check for conflicts with existing knowledge
3. Ask you to resolve conflicts (or create/merge automatically)
4. Commit and push to GitHub

Memories are Markdown files in `~/.auggie/memories/` with an `INDEX.md` that categorizes them.

### Rule Propagation

Sub-agents don't automatically inherit context. The rules.md file defines a propagation protocol—when spawning sub-agents, the orchestrator injects key instructions (shell syntax, memory access, credential handling).

### Shell Enforcement

By default, Auggie enforces **fish shell** syntax for all commands. This prevents bash/fish syntax mixing errors. Customize this in `rules.md` if you prefer a different shell.

## Directory Structure

```
~/.auggie/
├── rules.md                   # Core behavioral rules
├── creds                      # API keys (gitignored)
├── memories/
│   ├── INDEX.md               # Categorized memory index
│   └── YYYY-MM-DD_*.md        # Individual memories
├── role-instructions/
│   ├── fish-shell-syntax.md   # Shell reference
│   ├── senior-engineer.md     # Example role
│   └── ...
├── tools/
│   ├── fish-utils.fish        # Utility functions
│   └── ...
├── config/                    # Configuration files
└── standups/                  # Daily standup logs (optional)
```

## Customization

After initial setup, personalize by:

- Adding memories: `"Remember that I prefer TypeScript over JavaScript"`
- Creating roles: `"Create a role instruction file for security-reviewer"`
- Adding tools: Scripts in `~/.auggie/tools/` are available to agents
- Editing rules: Modify `rules.md` for different shell, git behavior, etc.

## Maintenance

Periodically ask your AI to:

```
Review my ~/.auggie/memories/ for conflicts or outdated information.
```

```
Consolidate all memories related to [TOPIC] into a single file.
```

## FAQ

**Q: Does this work with other AI assistants?**  
A: The prompts are designed for Augment but work with any AI that can read/write files and run shell commands.

**Q: What if I use bash instead of fish?**
A: See the [Switching to Bash](#switching-to-bash) section below.

**Q: How do I share memories across a team?**  
A: Use a shared GitHub repo, or maintain a `team-memories/` directory that individuals can pull from.

## Switching to Bash

Auggie defaults to fish shell, but you can switch to bash:

### 1. Modify `rules.md`

Replace the fish enforcement section with:

```markdown
# Bash Shell Enforcement (MANDATORY)

**Bash shell is REQUIRED for ALL shell commands.** There are no exceptions.

- All `launch-process` commands MUST use bash shell syntax exclusively
- Fish syntax (e.g., `set` for variables, `; and`/`; or` chaining) is STRICTLY PROHIBITED
- See `$HOME/.auggie/role-instructions/bash-shell-syntax.md` for detailed syntax reference
- When utility functions are needed, source them with: `source $HOME/.auggie/tools/bash-utils.sh`
```

### 2. Create `role-instructions/bash-shell-syntax.md`

Replace `fish-shell-syntax.md` with a bash reference. Include:

- Variable assignment: `VAR="value"` (no spaces around `=`)
- Command substitution: `$(command)` or backticks
- Conditionals: `[[ ]]` for tests, `if/then/fi`
- Loops: `for x in list; do ...; done`
- Arrays: `arr=(a b c)`, access with `${arr[0]}` (0-indexed)
- String manipulation: `${var#pattern}`, `${var%pattern}`, `${var/find/replace}`
- Here-docs: `<<EOF ... EOF`
- Common gotchas: quoting, word splitting, glob expansion

### 3. Create `tools/bash-utils.sh`

Replace `fish-utils.fish` with bash equivalents:

```bash
#!/usr/bin/env bash

# Source credentials
bash_source_creds() {
    source "$HOME/.auggie/creds"
}

# Require a variable to be set
bash_require_var() {
    local var_name="$1"
    if [[ -z "${!var_name:-}" ]]; then
        echo "ERROR: Required variable $var_name is not set" >&2
        return 1
    fi
}

# Log with timestamp
bash_log() {
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] $*"
}
```

### 4. Update onboarding prompts

When running the onboarding prompts, tell the AI:

```
Use bash shell instead of fish for all commands and examples.
Create bash-shell-syntax.md instead of fish-shell-syntax.md.
Create bash-utils.sh instead of fish-utils.fish.
```

### 5. Update sub-agent propagation

In `rules.md`, change the sub-agent instruction pattern to reference bash:

```
- Use bash shell syntax for all commands (no fish). Reference: ~/.auggie/role-instructions/bash-shell-syntax.md
```

## License

MIT — Use however you like.

