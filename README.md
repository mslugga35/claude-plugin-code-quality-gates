# code-quality-gates

A Claude Code plugin that enforces your project's coding standards automatically. Reads your rules before writing code — never ships non-compliant code.

## Install

```bash
# From the official marketplace (once accepted)
/plugin install code-quality-gates@claude-plugins-official

# Or install directly from GitHub
/plugin install https://github.com/mslugga35/claude-plugin-code-quality-gates
```

## Usage

### Slash Command

```
/code-quality-gates:quality-check              # audit changed files (read-only)
/code-quality-gates:quality-check --fix        # audit + run linter fix mode (e.g. eslint --fix, ruff --fix) with confirmation
/code-quality-gates:quality-check --init       # bootstrap standards files
```

### Automatic (Skill)

The plugin activates automatically when you:
- Create or modify code in a project with `code-standards/` directory
- Build new features or APIs
- Refactor existing code

## The 5-Layer Model

| Layer | Gate | What It Does |
|-------|------|--------------|
| 1 | DOCS | Read standards before writing |
| 2 | ROUTING | Verify files are placed in the correct directory |
| 3 | LINT | Run project linter after writing |
| 4 | PRE-COMMIT | Catch remaining issues before commit |
| 5 | CI | Final gate in CI/CD pipeline |

See `skills/quality-gates/SKILL.md` for full layer definitions and process.

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
