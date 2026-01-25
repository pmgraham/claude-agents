---
name: google-adk
description: Use when building AI agents with Google ADK (Agent Development Kit), creating Gemini agents, implementing multi-agent systems, or working with ADK tools and workflows. Covers installation, agent types, function tools, callbacks, sessions, and deployment.
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
  - Glob
  - Grep
  - WebFetch
  - WebSearch
---

# Google ADK Agent Development

This skill provides comprehensive guidance for building AI agents using Google's Agent Development Kit (ADK).

## IMPORTANT: Always Verify with Current Documentation

**Before providing any guidance, ALWAYS use WebFetch to check the official ADK documentation at https://google.github.io/adk-docs/ for the most current information.**

The ADK is actively developed and APIs may change. When answering questions:

1. First use WebFetch on the relevant documentation page (e.g., `https://google.github.io/adk-docs/agents/` for agent questions)
2. Cross-reference the fetched content with the reference material in this skill
3. If there are discrepancies, prefer the official documentation
4. Inform the user if you find updated information that differs from cached knowledge

Key documentation pages to check:
- Installation/Quickstart: `https://google.github.io/adk-docs/get-started/quickstart/`
- Agents: `https://google.github.io/adk-docs/agents/`
- Tools: `https://google.github.io/adk-docs/tools/`
- Multi-agent systems: `https://google.github.io/adk-docs/agents/multi-agents/`
- Callbacks: `https://google.github.io/adk-docs/callbacks/`

## When to Use This Skill

- User mentions "ADK", "Agent Development Kit", or "Google ADK"
- User wants to build AI agents with Gemini or other LLMs
- User asks about multi-agent systems or agent orchestration
- User needs help with ADK tools, callbacks, or workflows
- User mentions ADK-specific terms like `LlmAgent`, `SequentialAgent`, `ParallelAgent`, `LoopAgent`

## Quick Start

### Installation
```bash
# Python
pip install google-adk

# TypeScript
npm install @google/adk @google/adk-devtools
```

### Minimal Agent (Python)
```python
from google.adk import Agent

def greet(name: str) -> dict:
    """Greet a user by name."""
    return {"message": f"Hello, {name}!"}

root_agent = Agent(
    name="greeter",
    model="gemini-2.0-flash",
    instruction="You are a friendly greeter. Use the greet tool to say hello.",
    tools=[greet],
)
```

### Run the Agent
```bash
adk run my_agent      # CLI
adk web               # Web UI at http://localhost:8000
```

## For Detailed Documentation

Read the comprehensive reference at: `~/.claude/skills/google-adk/reference.md`

This reference includes:
- Complete agent configuration options
- Function tool best practices with examples
- Multi-agent system patterns
- Workflow agents (Sequential, Parallel, Loop)
- Callbacks for observability and guardrails
- Session and state management
- All supported models and pre-built tools

## Key Concepts Summary

### Agent Types
1. **LLM Agents**: Use LLMs for dynamic reasoning (non-deterministic)
2. **Workflow Agents**: Deterministic execution patterns
   - `SequentialAgent`: One after another
   - `ParallelAgent`: Concurrent execution
   - `LoopAgent`: Iterative until condition met
3. **Custom Agents**: Extend `BaseAgent` for specialized needs

### Essential Configuration
```python
Agent(
    name="unique_name",           # Required - unique identifier
    model="gemini-2.0-flash",     # Required - LLM model
    instruction="...",            # Required - system prompt
    description="...",            # Optional - for multi-agent routing
    tools=[...],                  # Optional - available tools
    sub_agents=[...],             # Optional - child agents
)
```

### Tool Definition Pattern
```python
def my_tool(param: str, optional_param: int = 0) -> dict:
    """Tool description for the LLM.

    Args:
        param: Required parameter description.
        optional_param: Optional with default value.

    Returns:
        Dictionary with status and results.
    """
    return {"status": "success", "result": "..."}
```

Always return dictionaries with status indicators for LLM comprehension.

## Official Documentation

https://google.github.io/adk-docs/
