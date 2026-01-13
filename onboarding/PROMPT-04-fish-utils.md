# PROMPT 04: Create Fish Utility Functions

Copy and paste this prompt to create common fish utility functions.

---

## Prompt

```
Create ~/.auggie/tools/fish-utils.fish (~120 lines) with common utility functions for fish shell scripts.

**Header**:
#!/usr/bin/env fish
# Fish Shell Utility Functions
# Source this file: source ~/.auggie/tools/fish-utils.fish

**Functions** (each with usage comment and example):

1. **fish_source_creds** - Source ~/.auggie/creds if it exists, warn if not found

2. **fish_require_var** VAR_NAME [error_message] - Check if variable is set, return 1 with error if not

3. **fish_log** "message" [level] - Log with timestamp in format [YYYY-MM-DD HH:MM:SS] [LEVEL] message

4. **fish_confirm** "prompt" - Ask y/n, return 0 for yes, 1 for no, loop on invalid input

5. **fish_json_get** "json_string" ".path" - jq wrapper, check if jq installed first

6. **fish_file_exists** "/path" - Return 0 if file exists

7. **fish_dir_exists** "/path" - Return 0 if directory exists

8. **fish_is_empty** "$var" - Return 0 if empty string

9. **fish_trim** "string" - Trim whitespace using `string trim`

10. **fish_join** "delimiter" items... - Join list elements with delimiter

Each function should:
- Have a comment block with description, usage, and example
- Use proper fish syntax (set -l for local vars)
- Return appropriate exit codes
- Write errors to stderr (&2)

Use fish shell syntax for all commands (no bash). Do NOT commit after creation.
```

---

## Expected Result

- ~/.auggie/tools/fish-utils.fish created with all utility functions
- File NOT committed (user will review first)

