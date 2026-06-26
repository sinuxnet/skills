---
name: import-knowledge-n-commit
description: Import knowledge into a markdown Knowledge Repo and auto-commit the result. Use when the user wants to document a story or file and immediately commit — or says "import and commit", "save and commit", "document this and commit".
---

# import-knowledge-n-commit

Identical to `/import-knowledge` through Step 7. Replaces Step 8: instead of suggesting a commit, execute it.

Run `/import-knowledge` Steps 1–7 exactly as written there.

Then, after all File Operations are applied:

```bash
git add <list of changed files>
git commit -m "import: <topic> → <file(s) changed>"
```

Report the commit hash and message to the user.
