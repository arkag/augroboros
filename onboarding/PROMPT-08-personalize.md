# PROMPT 08: Personalize Your Setup

Copy and paste this prompt to add personal preferences and first memories.

---

## Prompt

```
Let's personalize my ~/.auggie/ setup with my preferences.

1. **Create a user preference memory file:**
   ~/.auggie/memories/YYYY-MM-DD_user-preferences.md

   Include:
   - Preferred name: [YOUR NAME]
   - Timezone: [YOUR TIMEZONE]
   - Default project/team: [YOUR PROJECT]
   - Communication preferences (e.g., "keep responses concise", "prefer detailed explanations")

2. **Update INDEX.md** to include this file in User Preferences category

3. **Create a "remember" instruction test:**
   Remember that when I say "done for the day", you should offer to create a standup summary.

4. **Verify the Memory Creation Protocol works:**
   - This should trigger the protocol
   - Search for related memories
   - Create new memory file if no conflicts
   - Update INDEX.md
   - Commit and push

5. **Final setup verification:**
   ```fish
   ls -la ~/.auggie/
   git -C ~/.auggie status
   git -C ~/.auggie remote -v
   git -C ~/.auggie log --oneline -5
   ```

Commit all new files and push to GitHub.
```

---

## After This Prompt

Your ~/.auggie/ system is fully set up! Going forward:

- Tell the AI to "remember" things and it will persist them
- Conflicts will be detected and you'll be asked to resolve them
- All changes sync to GitHub automatically
- Sub-agents will receive proper context via the propagation rules

---

## Maintenance Tips

1. **Periodically review memories:**
   ```fish
   # Ask the AI to review
   echo "Review my memory system for conflicts or outdated information."
   ```

2. **Create new roles as needed:**
   ```fish
   # Ask the AI to create a role
   echo "Create a new role instruction file for [ROLE NAME] with these behaviors: ..."
   ```

3. **Consolidate related memories:**
   ```fish
   # Ask the AI to consolidate
   echo "Consolidate all memories related to [TOPIC] into a single file."
   ```

4. **Manual verification commands:**
   ```fish
   # Check current state
   ls -la ~/.auggie/
   git -C ~/.auggie status
   git -C ~/.auggie log --oneline -10

   # Sync with remote
   git -C ~/.auggie pull --rebase
   git -C ~/.auggie push
   ```

