# Global Claude Code Rules

## Git Workflow

**NEVER merge a PR without explicit user approval.**

1. Create the PR
2. Wait for user to test
3. Wait for explicit "merge it" or similar instruction from user
4. Only then merge

This rule applies to ALL repositories.

- Feature branches for all changes
- Clear commit messages

## Database Standards

- Never hardcode reference/lookup data in code
- Store reference data in database tables (use `helpers` schema)
- No abbreviations in schema/table/column names
- Always run migrations against actual database, not just create files

## Code Quality

- No unnecessary abbreviations - prefer readable names
- Clean, organized project structure
- Remove dead code (don't comment it out)
