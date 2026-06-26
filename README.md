[![skills.sh](https://skills.sh/b/sinuxnet/skills)](https://skills.sh/sinuxnet/skills)

# Sinuxnet Skills

Agent skills for Claude Code focused on company knowledge management — import stories, commands, and files into a structured markdown knowledge repo your whole team can query.

## Quickstart (30-second setup)

1. Run the skills.sh installer:
   ```
   npx skills@latest add sinuxnet/skills
   ```
2. Open your company knowledge repo in Cursor or Claude Code.
3. Start importing knowledge:
   ```
   /import-knowledge We migrated our web server from Nginx to Caddy last week. The config lives at /etc/caddy/Caddyfile.
   ```

## Skills

### `/import-knowledge`

Ingests a story, command, or file path and places the knowledge into the right markdown documents in your repo. Reads your existing structure first, proposes a plan, waits for your approval, then executes.

- Accepts prose or any text-based file (`.md`, `.txt`, `.yaml`, `.sh`, `.log`, …)
- Auto-bootstraps an empty repo with a seed structure on first run
- Auto-redacts credentials, passwords, and secret tokens before writing
- Adds `- [ ]` TODOs for incomplete facts, `<!-- changed: -->` comments for conflicts
- Adds YAML frontmatter (`last_updated`, `tags`, `source`) to every touched file
- Suggests a structured `git commit` message; you run it yourself

### `/import-knowledge-n-commit`

Identical to `/import-knowledge` but auto-commits after applying all changes.

## Why These Skills Exist

Company knowledge lives in people's heads, Slack threads, and one-off runbooks that nobody updates. This skill turns the natural act of telling a story — "here's what I did, here's what I learned" — into structured, queryable markdown that survives team turnover.

Clone your knowledge repo, open it in any Claude Code-compatible editor, and ask questions. The repo becomes your company's memory.

## License

MIT
