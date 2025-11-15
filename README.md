# Claude Agents

A collection of specialized agent definitions for Claude Code to enhance development workflows.

## Overview

This repository contains custom agent definitions that provide specialized capabilities for various development tasks. Each agent is designed to handle specific aspects of software development with expertise and best practices.

## Learn More

For detailed information on creating and configuring agents in Claude Code, see the [official Claude Code subagents documentation](https://code.claude.com/docs/en/sub-agents#using-the-agents-command-recommended).

# Claude Agents Setup

## Installation

To use these agents in Claude Code, copy the agent files to your Claude Code configuration directory:

```bash
# Create the agents directory if it doesn't exist
mkdir -p ~/.claude/agents

# Copy all agents to your Claude Code directory
cp agents/*.md ~/.claude/agents/
```

Alternatively, you can symlink individual agents:

```bash
# Symlink specific agents
ln -s /path/to/claude-agents/agents/git-workflow-manager.md ~/.claude/agents/
ln -s /path/to/claude-agents/agents/project-tracker.md ~/.claude/agents/
```

## Available Agents

### Git Workflow Manager
- **Location**: `agents/git-workflow-manager.md`
- **Purpose**: Manages git operations with intelligent commit message generation, automated push workflows, and best practices enforcement.

### GitHub Manager
- **Location**: `agents/github-manager.md`
- **Purpose**: Handles GitHub-specific operations including repository management, pull requests, issues, and collaboration workflows.

### Project Tracker
- **Location**: `agents/project-tracker.md`
- **Purpose**: Manages project tasks, tracks progress, and organizes development workflows.

### JavaScript Expert Code Reviewer
- **Location**: `agents/javascript-expert-code-reviewer.md`
- **Purpose**: Provides expert-level code reviews for JavaScript projects with focus on best practices, performance, and security.

### Python Expert Code Reviewer
- **Location**: `agents/python-expert-code-reviewer.md`
- **Purpose**: Provides expert-level code reviews for Python projects with focus on best practices, performance, and security.

## Usage

These agents are designed to be used with Claude Code. Reference them in your CLAUDE.md file or invoke them directly when needed for specific development tasks.

## Project Configuration

The `CLAUDE.md` file in this repository provides instructions for using these agents effectively in your development workflow.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
