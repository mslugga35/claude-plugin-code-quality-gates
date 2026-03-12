---
name: code-quality-gates
description: This skill should be used when creating or modifying code in a project that has coding standards, conventions, or quality rules. Triggers on new feature implementation, code refactoring, API development, or when a project has a code-standards/ directory, RULES.md, CONVENTIONS.md, or SECURITY.md.
version: 1.0.0
---

# Code Quality Gates

Enforce project coding standards automatically. Reads your project's rules before writing code — never ships non-compliant code.

## The 5-Layer Model

| Layer | Gate | What It Does |
|-------|------|--------------|
| 1 | DOCS | Read standards before writing (RULES.md, CONVENTIONS.md, SECURITY.md) |
| 2 | ROUTING | Check new files are placed in the correct directory per CONVENTIONS.md |
| 3 | LINT | Run project linter after writing (ESLint, Ruff, RuboCop, etc.) |
| 4 | PRE-COMMIT | Catch remaining issues before commit (hooks) |
| 5 | CI | Final gate in CI/CD pipeline |

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
2. Read `CONVENTIONS.md` — know the project patterns and where files should go
3. Read `SECURITY.md` — know the security requirements
4. Check existing code in the same directory for style consistency

### Step 3: Route the Code (Layer 2)

Before creating new files, verify placement:
- Check CONVENTIONS.md for directory structure rules
- Look at existing files in the target directory to match patterns
- If adding a new component/module, follow the established naming and location conventions

### Step 4: Write Compliant Code

- Follow naming conventions from RULES.md
- Use established patterns from CONVENTIONS.md
- Validate inputs at system boundaries per SECURITY.md
- Use the project's preferred error handling pattern
- Match the style of surrounding code

### Step 5: Validate (Layer 3)

Run the project's quality checks. Only run tools that are configured for the project:

```bash
# JavaScript/TypeScript — check for config before running
test -f .eslintrc* && npm run lint 2>&1
test -f tsconfig.json && npx tsc --noEmit 2>&1
npm test -- --related 2>&1

# Python — check for config before running
test -f pyproject.toml && ruff check . --fix 2>&1
test -f mypy.ini -o -f pyproject.toml && mypy . 2>&1
pytest -x --tb=short 2>&1

# Ruby
test -f .rubocop.yml && bundle exec rubocop -a 2>&1
bundle exec rspec --fail-fast 2>&1
```

### Step 6: Exception Handling

If a rule must be broken:
- Add inline suppression with reason: `// eslint-disable-next-line rule-name -- reason`
- Keep exceptions minimal
- Document exceptions in the PR description

## Bootstrapping Standards for New Projects

If a project has no `code-standards/` directory, offer to create one with starter templates:

**RULES.md** — naming, imports, error handling policies
**CONVENTIONS.md** — file structure, data flow, state management patterns
**SECURITY.md** — input validation, auth, XSS prevention requirements

Analyze existing code in the project to infer current conventions and populate the templates accordingly.

## What This Skill Does NOT Do

- Does not replace your CI pipeline
- Does not enforce rules not written in your project's docs
- Does not auto-fix security vulnerabilities (flags them for review)
