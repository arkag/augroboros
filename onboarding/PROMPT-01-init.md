# PROMPT 01: Initialize Repository

Copy and paste this prompt to your AI assistant to initialize the .auggie repository.

---

## Prompt

```
I want to set up a personal AI assistant memory system at ~/.auggie/. This will be a git repository that syncs to GitHub.

Please do the following:

1. Create the directory structure:
   ~/.auggie/
   ├── memories/
   ├── role-instructions/
   ├── tools/
   ├── config/
   └── standups/

2. Initialize a git repository in ~/.auggie/

3. Create a .gitignore file with:
   - .DS_Store
   - *.pyc
   - __pycache__/
   - .env
   - *.log
   - creds

4. Create an initial README.md explaining this is a personal AI assistant configuration repository.

5. Make an initial commit with the message "Initial commit: auggie repository structure"

After this is done, I'll need to manually:
- Create a GitHub repository (public or private)
- Add the remote: git remote add origin git@github.com:USERNAME/auggie.git
- Push: git push -u origin main

Use fish shell syntax for all commands.
```

---

## Expected Result

- Directory structure created at ~/.auggie/
- Git repository initialized
- .gitignore and README.md created
- Initial commit made
- Ready for GitHub remote to be added

