---
name: code-quality-gates
description: This skill should be used when creating or modifying code in a project that has coding standards, conventions, or quality rules. Triggers on new feature implementation, code refactoring, API development, or when a project has a code-standards/ directory, RULES.md, CONVENTIONS.md, or SECURITY.md.
version: 1.0.0
---

# Code Quality Gates

Enforce project coding standards automatically. Reads your project's rules before writing code — never ships non-compliant code.

## The 5-Layer Model

```
Layer 1: DOCS      — Read standards before writing (RULES.md, CONVENTIONS.md, SECURITY.md)
Layer 2: ROUTING   — Route code through the right patterns (file structure, data flow)
Layer 3: LINT      — Run project linter after writing (ESLint, Ruff, RuboCop, etc.)
Layer 4: PRE-COMMIT — Catch remaining issues before commit (hooks)
Layer 5: CI        — Final gate in CI/CD pipeline
```

## Process

### Step 1: Find Standards

Look for quality docs in the project:

```
<project>/
  code-standards/          # Dedicated standards directory
    RULES.md               # Hard policies (naming, imports, error handling)
    CONVENTIONS.md         # Project patterns (file structure, data flow, state)
    SECURITY.md            # Security requirements (validation, auth, XSS)
  CLAUDE.md                # Project-level Claude instructions
  .eslintrc.*              # Linter config
  .prettierrc              # Formatter config
  pyproject.toml           # Python project config (ruff, black, mypy)
```

If no `code-standards/` directory exists, check `CLAUDE.md` for project conventions.

### Step 2: Read Before Writing

Before writing any code:
1. Read `RULES.md` — know what's banned
2. Read `CONVENTIONS.md` — know the project patterns
3. Read `SECURITY.md` — know the security requirements
4. Check existing code in the same directory for style consistency

### Step 3: Write Compliant Code

- Follow naming conventions from RULES.md
- Use established patterns from CONVENTIONS.md
- Validate inputs at system boundaries per SECURITY.md
- Use the project's preferred error handling pattern
- Match the style of surrounding code

### Step 4: Validate

Run the project's quality checks:

```bash
# JavaScript/TypeScript
npm run lint 2>&1 || npx eslint --fix .
npm run typecheck 2>&1 || npx tsc --noEmit
npm test -- --related

# Python
ruff check . --fix || python -m flake8
mypy . || true
pytest -x --tb=short

# Ruby
bundle exec rubocop -a
bundle exec rspec --fail-fast
```

### Step 5: Exception Handling

If a rule must be broken:
- Add inline suppression with reason: `// eslint-disable-next-line rule-name -- reason`
- Keep exceptions minimal
- Document exceptions in the PR description

## Bootstrapping Standards for New Projects

If a project has no `code-standards/` directory, offer to create one:

```markdown
# code-standards/RULES.md

## Naming
- Files: kebab-case
- Functions: camelCase
- Classes: PascalCase
- Constants: UPPER_SNAKE_CASE

## Imports
- Group: stdlib, external, internal, relative
- No circular imports
- Prefer named exports over default

## Error Handling
- Never swallow errors silently
- Use typed errors where possible
- Log errors with context (what failed, why, what to do)
```

## What This Skill Does NOT Do

- Does not replace your CI pipeline
- Does not enforce rules not written in your project's docs
- Does not auto-fix security vulnerabilities (flags them for review)
