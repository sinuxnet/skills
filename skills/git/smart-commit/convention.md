# Commit Convention

Commit messages communicate **why** a change exists and what **engineering intent** it serves.

## Message Format

```text
<type>(<scope>): <subject>

[optional body]
```

## Commit Types

- **feat**: New functionality
- **fix**: Bug fix
- **refactor**: Internal improvement, no behavior change
- **perf**: Performance improvement
- **docs**: Documentation only
- **test**: Tests only
- **build**: Build system or dependency changes
- **ci**: CI/CD changes
- **chore**: Maintenance work
- **style**: Formatting only
- **revert**: Revert a previous commit

## Scopes

Use the most specific scope possible. Examples:
- `auth`, `api`, `frontend`, `ui`, `chat`, `rag`, `retrieval`
- `ingestion`, `qdrant`, `tenant`, `agent`, `pipeline`, `worker`
- `docker`, `deployment`, `k8s`, `observability`, `security`, `config`, `docs`

## Subject Rules

The subject must:
- Use imperative mood
- Be lowercase
- Have no trailing period
- Be concise and describe intent

**Good:** `feat(rag): add hybrid retrieval strategy`
**Bad:** `feat(rag): Added hybrid retrieval strategy.`

## Body Guidelines

Add a body when the reason for change is not obvious.

Recommended structure:

```text
Why:
- explain the problem

Changes:
- summarize important modifications

Impact:
- deployment effects
- migrations
- compatibility concerns
```

## Breaking Changes

Mark explicitly with `!` or `BREAKING CHANGE:` footer.

```text
feat(api)!: redesign authentication endpoints
```

or

```text
feat(api): redesign authentication endpoints

BREAKING CHANGE:
clients must migrate to v2 endpoints
```

## Granularity

One commit = one logical change, independently revertable.

- A refactor enabling a feature is a separate commit
- A fix and a new feature in the same module are separate commits
- Mention tools in commit messages — describe the actual change
