# Sinuxnet Skills

## 1.0.0

### Major Changes

- **`import-knowledge`**: New skill — ingests a story, command, or file and distributes the knowledge into the right markdown documents in a Knowledge Repo. Proposes a plan before executing. Auto-redacts credentials. Adds TODOs for gaps, conflict comments for contradictions, and YAML frontmatter to every touched file.

- **`import-knowledge-n-commit`**: Variant of `import-knowledge` that auto-commits after applying all changes.
