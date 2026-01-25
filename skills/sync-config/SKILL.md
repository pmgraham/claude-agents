# Sync Claude Configuration

This skill syncs Claude Code configuration between local (~/.claude/) and the GitHub repo (pmgraham/claude-agents).

## Usage
- `/sync-config push` - Push local config to GitHub
- `/sync-config pull` - Pull latest config from GitHub
- `/sync-config` (no args) - Pull latest (used at session start)

## Push (local -> GitHub)
When pushing, copy these files to the repo and commit/push:
1. ~/.claude/CLAUDE.md -> /home/pmgraham/projects/claude-agents/CLAUDE.md
2. ~/.claude/skills/* -> /home/pmgraham/projects/claude-agents/skills/
3. ~/.claude/agents/* -> /home/pmgraham/projects/claude-agents/agents/

```bash
# Copy files
cp ~/.claude/CLAUDE.md /home/pmgraham/projects/claude-agents/
cp -r ~/.claude/skills/* /home/pmgraham/projects/claude-agents/skills/
cp -r ~/.claude/agents/* /home/pmgraham/projects/claude-agents/agents/ 2>/dev/null || true

# Commit and push
cd /home/pmgraham/projects/claude-agents
git add -A
git commit -m "Sync Claude configuration" || echo "No changes to commit"
git push origin main || git push origin master
```

## Pull (GitHub -> local)
When pulling, fetch latest and copy to local:

```bash
# Pull latest
cd /home/pmgraham/projects/claude-agents
git pull origin main || git pull origin master

# Create directories if needed
mkdir -p ~/.claude/skills ~/.claude/agents

# Copy files
cp /home/pmgraham/projects/claude-agents/CLAUDE.md ~/.claude/CLAUDE.md
cp -r /home/pmgraham/projects/claude-agents/skills/* ~/.claude/skills/
cp -r /home/pmgraham/projects/claude-agents/agents/* ~/.claude/agents/ 2>/dev/null || true
```

After pulling, check if any files changed. If yes, advise user: "Configuration updated from GitHub. Please restart Claude Code to apply changes."
