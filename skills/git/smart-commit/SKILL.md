---
name: smart-commit
description: Analyze git changes, group them into atomic commits following the project's commit convention, and commit after approval.
disable-model-invocation: true
---

# smart-commit

Read every pending change in the working tree, group them into atomic commits that follow the embedded commit convention, show the full plan, and execute on approval.

> **Convention reference:** `convention.md` (embedded in skill directory)
> Load this file before Step 3. Every type, scope, subject rule, and body guideline lives there.

---

## Steps

### 1. Collect

Run `git status --short` and `git diff HEAD` (including untracked files via `git ls-files --others --exclude-standard`).

If the user's invocation named any files to exclude, drop them now and never revisit them.

**Done when:** you hold the full list of changed, staged, and untracked files — minus any explicit exclusions.

---

### 2. Load convention

Read `convention.md` from the skill directory.

**Done when:** you know every valid type, scope rule, subject format, body structure, and breaking-change marker.

---

### 3. Analyze

Read the diff for every file from Step 1 (`git diff HEAD <file>` for tracked files; read the file directly for untracked ones).

For each file, identify: what changed, why it likely changed, and what behavior it affects.

**Done when:** every file has a clear understanding of its intent — not just "what lines changed" but "what engineering purpose this serves."

---

### 4. Group atomically

Cluster files into atomic commits. One commit = one logical change, independently revertable.

Rules:
- Primary signal: diff content and engineering intent
- Secondary signal: file paths (same directory, same layer, same feature)
- A refactor that enables a feature is a separate commit from the feature itself
- A fix and a new feature in the same module are separate commits

**Done when:** every file belongs to exactly one commit group, and no group mixes unrelated intents.

---

### 5. Triage

For any file whose purpose you cannot confidently classify — ambiguous diff, mixed concerns, unclear scope — **stop and surface it to the user.**

Show:
- The file path
- The relevant diff excerpt
- What makes it ambiguous

Wait for the user to clarify before proceeding. Do not guess and proceed.

**Done when:** every file has a resolved classification. No ambiguities remain.

---

### 6. Order

Sort commit groups by logical dependency:
- Foundations first: refactors, renames, config changes that others depend on
- Then: fixes
- Then: features
- Then: docs, tests, chores

If two groups are independent, order by type in the sequence above.

**Done when:** every commit group has a position in the sequence.

---

### 7. Present plan

Show the full commit plan — all commits at once — before touching git.

Format each commit as:

```
Commit N: <type>(<scope>): <subject>
  Files: <file1>, <file2>, ...
  Body: <Why / Changes / Impact> — or "(none)" if not warranted
```

Include a body only when the change spans multiple files with non-obvious impact, introduces behavior visible to callers, or has deployment or migration consequences.

**Done when:** every commit group is displayed with its message and file list.

---

### 8. Gate

Ask: **"Proceed with these commits? (yes/no)"**

On **no**: stop. Do not commit anything.

On **yes**: continue to Step 9.

**Done when:** the user has answered.

---

### 9. Execute

For each commit group in order:

```bash
git add <files>
git commit -m "<type>(<scope>): <subject>" [-m "<body>"]
```

Commit body: pass as a second `-m` flag when warranted (from Step 7).

**Done when:** every commit group has been committed and `git log --oneline -N` confirms all N commits landed with the correct messages.
