# Global Claude Code Rules

## SDLC (Software Development Lifecycle)

**NEVER make changes directly on main/master branches** (except for documentation/markdown files).

1. Create a feature branch, bugfix branch, or whatever is appropriate
2. Make changes on the branch
3. Push to GitHub
4. Create a PR
5. Flag user when ready for review
6. Wait for explicit approval before merging

## Database Standards

### Schema Changes
- **NEVER make schema changes without express user consent**
- Explain what changes are being made and why before proceeding
- Wait for approval before executing any DDL (CREATE, ALTER, DROP)

### Naming Conventions
- **Table names**: plural, all lowercase, snake_case (e.g., `stop_words`, `user_accounts`)
- **Column names**: all lowercase, snake_case (e.g., `created_at`, `user_id`)
- **Date columns**: use `_on` suffix (e.g., `created_on`, `updated_on`, `sold_on`)
- **Datetime columns**: use `_at` suffix (e.g., `created_at`, `updated_at`, `sold_at`)
- **Boolean columns**: use `is_` prefix (e.g., `is_included`, `is_holiday`, `is_excluded`)
- When in doubt, opt for **clarity over brevity**

### Data Integrity
- **NEVER alter data without express user consent**
- If a data formatting issue is found, flag it for user review
- Do not INSERT, UPDATE, or DELETE data without approval

### Query and Migration Files
- **NEVER embed SQL queries in code** - store them in separate files (.sql, .md, or .txt)
- **Migrations** should be stored separately from application queries
- Organize database files distinctly:
  - `migrations/` - schema changes (DDL)
  - `queries/` - application read/lookup queries (DQL)
- Reference queries from code, don't inline them

### Reference Data
- Never hardcode reference/lookup data in code
- Store reference data in database tables (use `helpers` schema)
- Always run migrations against actual database, not just create files

## Code Quality

### General
- No unnecessary abbreviations - prefer readable names
- Clean, organized project structure
- Remove dead code (don't comment it out)

### Python
- Follow **PEP8** coding standards

### Golang (API Web Services)
- Follow the **MVC pattern** (Models, Views/Handlers, Controllers/Services)
