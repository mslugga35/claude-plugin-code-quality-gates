# code-quality-gates

A Claude Code plugin that enforces your project's coding standards automatically. Reads your rules before writing code — never ships non-compliant code.

## Install

```bash
/plugin install code-quality-gates@claude-plugins-official
```

## Usage

### Slash Command

```
/code-quality-gates:quality-check
/code-quality-gates:quality-check --fix
/code-quality-gates:quality-check --init
```

### Automatic (Skill)

The plugin activates automatically when you:
- Create or modify code in a project with `code-standards/` directory
- Build new features or APIs
- Refactor existing code

## The 5-Layer Model

```
Layer 1: DOCS       Read standards before writing
Layer 2: ROUTING    Route code through the right patterns
Layer 3: LINT       Run project linter after writing
Layer 4: PRE-COMMIT Catch remaining issues before commit
Layer 5: CI         Final gate in CI/CD pipeline
```

## Project Standards

The plugin looks for quality docs in your project:

```
your-project/
  code-standards/
    RULES.md          # Naming, imports, error handling
    CONVENTIONS.md    # File structure, data flow, state management
    SECURITY.md       # Input validation, auth, XSS prevention
```

No standards yet? Run `/code-quality-gates:quality-check --init` to generate starter files based on your existing code patterns.

## Supported Languages

- JavaScript / TypeScript (ESLint, Prettier, tsc)
- Python (Ruff, Flake8, mypy)
- Ruby (RuboCop)

## License

MIT
