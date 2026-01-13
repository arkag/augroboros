# PROMPT 07: Set Up Credentials File

Copy and paste this prompt to set up the credentials file.

---

## Prompt

```
Create a fish-sourceable credentials file at ~/.auggie/creds.

Create the file with this exact structure:

#!/usr/bin/env fish
# Credentials File
# Source this file: source ~/.auggie/creds
#
# SECURITY: This file is gitignored. Never commit actual credentials.
#
# To add credentials:
# 1. Edit this file
# 2. Use: set -gx VAR_NAME "value"
# 3. For base64 encoded secrets: set -gx VAR_NAME (echo "secret" | base64 -d)

# Example credentials (replace with actual values):
# set -gx JIRA_API_TOKEN "your-token-here"
# set -gx SLACK_WEBHOOK_URL "https://hooks.slack.com/..."
# set -gx GITHUB_TOKEN "ghp_..."


After creating the file:
1. Verify that 'creds' is in .gitignore - if not, add it
2. Set secure file permissions: chmod 600 ~/.auggie/creds

Do NOT commit actual credentials - only template/structure.
```

---

## After This Prompt

The teammate should manually:
1. Edit ~/.auggie/creds with their actual credentials
2. Run `chmod 600 ~/.auggie/creds` to set secure permissions
3. Never commit the file with real values
4. Source the file: `source ~/.auggie/creds`

---

## Expected Result

- ~/.auggie/creds template created (fish-sourceable)
- File permissions set to 600 (owner read/write only)
- .gitignore updated to exclude creds if needed

