---
description: Run quality gates on current project — check standards compliance, lint, and test
argument-hint: [files...] [--fix] | --init
allowed-tools: [Bash, Read, Grep, Glob]
---

Run quality gates on the current project.

**Mode: `--init`** (creates standards files)
- Confirm with the user before creating any files: "About to create code-standards/ with RULES.md, CONVENTIONS.md, SECURITY.md. Proceed?"
- Only after user confirms, create the `code-standards/` directory with starter templates
- Analyze existing code to infer current conventions and populate the templates

**Mode: default** (read-only audit)
1. Find and read project standards (code-standards/, CLAUDE.md, linter configs)
2. Check specified files against standards. If no files specified, check `git diff --name-only` for changed files. If git diff is empty, inform the user there are no changed files to check.
3. Report violations as a table
4. Do NOT modify any files in this mode

**Mode: `--fix`** (audit + auto-fix)
1. Run the same audit as default mode
2. Show the violations table
3. Ask the user to confirm before making any changes
4. Only after confirmation, apply auto-fixes and re-run checks
