---
name: import-knowledge
description: Import knowledge into a markdown Knowledge Repo. Use when the user provides a story, command, or file they want documented — or says "import this", "document this", "remember this", "add this to the repo", "capture this", or "save this to the knowledge repo".
---

# import-knowledge

Ingest an Import Input (prose or file path) and place its knowledge into the right Knowledge Documents in this repo. A Knowledge Repo is a private git repo of markdown files representing company knowledge — infrastructure, people, decisions, runbooks, projects.

## Steps

### 1. Detect repo type and prepare docs/ directory

Check if this is a **coding repo** by looking for:
- `package.json`, `Cargo.toml`, `go.mod`, `pyproject.toml`, `setup.py`, `pom.xml`, `build.gradle`
- `.git/` directory with source code

If coding repo detected:
- Check if `docs/` directory exists
- If `docs/` does not exist, create it
- Use `docs/` as the playground for new knowledge files

If **documentation repo** (no coding markers found):
- Skill is free to place files anywhere in the repo structure

**Done when:** you've determined repo type and ensured `docs/` exists (if coding repo).

---

### 2. Read the Import Input

If the user gave a file path, read the file. Accept any text-based format: `.md`, `.txt`, `.yaml`, `.sh`, `.log`, etc. If prose, take it as-is.

**Done when:** you hold the full content of the Import Input.

---

### 3. Orient in the repo

Read `README.md` first, then any folder-level `README.md` or index files you find. Then run `find . -name "*.md" | head -100` (or `tree -L 3 --dirsfirst`) to map the full structure.

If the repo contains no structure (empty or a blank README), go to **Step 4**.

**Done when:** you have a clear map of where knowledge currently lives.

---

### 4. Bootstrap *(empty repo only)*

Create seed folders and a `README.md` for each:

```
README.md
infra/README.md
people/README.md
runbooks/README.md
projects/README.md
decisions/README.md
```

Note "Initialized base structure" in the Proposal.

**Done when:** seed structure exists and the repo is no longer empty.

---

### 5. Redact

Strip **credentials, passwords, and secret tokens** from the Import Input before writing anything. IPs are safe — do not redact them.

Record every redaction: what type of secret was removed and from what context. This appears in the Proposal.

**Done when:** no credentials, passwords, or secret tokens remain in any content destined for a file.

---

### 6. Extract and place

Extract every fact from the Import Input. Map each fact to a File Operation — create, edit, move, or delete a Knowledge Document. A single Import Input spanning multiple topics produces multiple File Operations; distribute facts to their natural homes.

**For coding repos:** All new knowledge files should be created under `docs/` and its subdirectories (e.g., `docs/guides/`, `docs/reference/`). Decide the best subdirectory based on content type — the LLM will make the placement decision.

**For documentation repos:** Place files naturally throughout the existing repo structure.

For each proposed edit to an existing document:
- Check for a **Conflict**: a new fact that contradicts current content. Flag it — it will get a `<!-- changed: reason -->` comment on execution.

For each fact where information is incomplete or missing:
- Plan a `- [ ]` TODO checkbox at the end of the target document.

**Done when:** every fact has a File Operation. Every Conflict is identified. Every gap has a TODO planned.

---

### 7. Propose

Show the user two parts and wait for approval before touching any file.

**Part A — File Operations:**
A bullet list of every create / edit / move / delete with the target path.

**Part B — Extracted Facts:**
Plain-language summary of what you understood: the knowledge being stored, any Conflicts detected (old vs. new), any Redactions made, any TODOs planned.

The user may approve as-is or correct inline ("yes but put this under `infra/k8s/`"). Apply all corrections before proceeding.

**Done when:** the user has approved the Proposal.

---

### 8. Execute

Apply every File Operation. For each created or edited Knowledge Document:

**Frontmatter** — add or update at the top of the file:
```yaml
---
last_updated: YYYY-MM-DD
tags: [inferred, from, content]
source: import-knowledge
---
```

**TODOs** — append at the bottom:
```md
## To do

- [ ] Missing fact one
- [ ] Missing fact two
```

**Conflicts** — add inline, immediately after the updated content:
```md
<!-- changed: old value was X — updated because [reason from Import Input] -->
```

**Done when:** all File Operations are applied, every modified document has correct frontmatter, every planned TODO and conflict comment is in place.

---

### 9. Suggest commit

Output the exact commands for the user to run:

```
git add <list of changed files>
git commit -m "import: <topic> → <file(s) changed>"
```

The user runs these themselves.

**Done when:** commit suggestion is shown.
