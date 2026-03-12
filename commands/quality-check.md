---
description: Run quality gates on current project — check standards compliance, lint, and test
argument-hint: [files...] [--fix] [--init]
allowed-tools: [Bash, Read, Grep, Glob, Edit, Write]
---

Run quality gates on the current project.

If `$ARGUMENTS` contains `--init`:
- Create a `code-standards/` directory with starter RULES.md, CONVENTIONS.md, SECURITY.md
- Analyze existing code to infer current conventions
- Populate the standards files based on what's already in the codebase

Otherwise:
1. Find and read project standards (code-standards/, CLAUDE.md, linter configs)
2. Check specified files (or all recently changed files via `git diff --name-only`) against standards
3. Report violations as a table
4. If `--fix` is present, auto-fix what's possible and re-run checks
