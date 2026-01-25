# Google ADK (Agent Development Kit) Skill

Use this skill when the user asks about building AI agents with Google ADK, wants to create agents, or mentions ADK, Agent Development Kit, or Gemini agents.

## Overview

Google ADK is a framework for developing and deploying AI agents. It is model-agnostic, deployment-agnostic, and built for compatibility with other frameworks. While optimized for Gemini, it supports multiple LLMs including Claude, Vertex AI, Ollama, vLLM, and LiteLLM.

**Documentation:** https://google.github.io/adk-docs/

## Installation

```bash
# Python
pip install google-adk

# TypeScript
npm install @google/adk @google/adk-devtools

# Go
go get google.golang.org/adk

# Java - use Maven/Gradle with appropriate dependencies
```

## Project Structure

### Python Project
```
my_agent/
├── __init__.py      # Makes it a package, exports root_agent
├── agent.py         # Agent definition with tools
└── .env             # API credentials
```

### TypeScript Project
```
my-adk-agent/
├── agent.ts         # Agent definition
├── .env             # API credentials
├── package.json
└── tsconfig.json
```

## Environment Setup

### Google AI Studio
```env
GOOGLE_GENAI_USE_VERTEXAI=FALSE
GOOGLE_API_KEY=your_api_key_here
```

### Vertex AI
```env
GOOGLE_GENAI_USE_VERTEXAI=TRUE
GOOGLE_CLOUD_PROJECT=your_project_id
GOOGLE_CLOUD_LOCATION=us-central1
```

## Core Agent Types

### 1. LLM Agents
Use LLMs for reasoning, planning, and dynamic decision-making. Non-deterministic and excellent for flexible, language-centric tasks.

### 2. Workflow Agents
Control execution flow in deterministic patterns without using an LLM:
- **SequentialAgent**: Executes sub-agents one after another
- **ParallelAgent**: Executes sub-agents concurrently
- **LoopAgent**: Repeats execution until a condition is met

### 3. Custom Agents
Extend `BaseAgent` for specialized implementations.

## Creating an LLM Agent (Python)

```python
# agent.py
from google.adk import Agent

def get_weather(city: str) -> dict:
    """Get weather for a city.

    Args:
        city: The city name to get weather for.

    Returns:
        Weather information for the city.
    """
    # Simulated response
    return {
        "status": "success",
        "city": city,
        "temperature": "72°F",
        "conditions": "Sunny"
    }

def get_current_time(timezone: str) -> dict:
    """Get current time for a timezone.

    Args:
        timezone: The timezone (e.g., 'America/New_York').

    Returns:
        Current time information.
    """
    from datetime import datetime
    import pytz

    tz = pytz.timezone(timezone)
    now = datetime.now(tz)
    return {
        "status": "success",
        "timezone": timezone,
        "time": now.strftime("%I:%M %p"),
        "date": now.strftime("%Y-%m-%d")
    }

root_agent = Agent(
    name="weather_time_agent",
    model="gemini-2.0-flash",
    description="An agent that provides weather and time information.",
    instruction="""You are a helpful assistant that provides weather and time information.

    When users ask about weather, use the get_weather tool.
    When users ask about time, use the get_current_time tool.

    Always be friendly and provide clear, concise responses.""",
    tools=[get_weather, get_current_time],
)
```

```python
# __init__.py
from .agent import root_agent
```

## Creating an LLM Agent (TypeScript)

```typescript
// agent.ts
import { Agent, FunctionTool } from '@google/adk';
import { z } from 'zod';

const getWeather = new FunctionTool({
  name: 'getWeather',
  description: 'Get weather for a city',
  parameters: z.object({
    city: z.string().describe('The city name to get weather for')
  }),
  execute: async ({ city }) => {
    return {
      status: 'success',
      city,
      temperature: '72°F',
      conditions: 'Sunny'
    };
  }
});

const getCurrentTime = new FunctionTool({
  name: 'getCurrentTime',
  description: 'Get current time for a timezone',
  parameters: z.object({
    timezone: z.string().describe('The timezone (e.g., America/New_York)')
  }),
  execute: async ({ timezone }) => {
    const now = new Date().toLocaleString('en-US', { timeZone: timezone });
    return {
      status: 'success',
      timezone,
      time: now
    };
  }
});

export const rootAgent = new Agent({
  name: 'weather_time_agent',
  model: 'gemini-2.5-flash',
  description: 'An agent that provides weather and time information.',
  instruction: `You are a helpful assistant that provides weather and time information.

When users ask about weather, use the getWeather tool.
When users ask about time, use the getCurrentTime tool.

Always be friendly and provide clear, concise responses.`,
  tools: [getWeather, getCurrentTime]
});
```

## LLM Agent Configuration Options

| Property | Required | Description |
|----------|----------|-------------|
| `name` | Yes | Unique identifier (avoid reserved names like "user") |
| `model` | Yes | LLM model identifier (e.g., "gemini-2.0-flash") |
| `instruction` | Yes | System prompt guiding agent behavior |
| `description` | No | Used for routing in multi-agent systems |
| `tools` | No | List of tools the agent can use |
| `sub_agents` | No | Child agents for multi-agent systems |
| `input_schema` | No | Expected input format (Pydantic model) |
| `output_schema` | No | Expected output format (Pydantic model) |
| `output_key` | No | Key to store final response in session state |
| `include_contents` | No | Whether to include conversation history ("default" or "none") |
| `generate_content_config` | No | LLM parameters (temperature, max_tokens, etc.) |

## Function Tools Best Practices

1. **Use type hints** - ADK uses them to generate schemas
2. **Write docstrings** - Describe what the function does and its parameters
3. **Return dictionaries** - Include status indicators like "success", "error"
4. **Minimize parameters** - Keep tools simple and focused
5. **Use simple types** - Prefer primitives over custom classes

### Optional Parameters
```python
def search_flights(
    destination: str,              # Required
    departure_date: str,           # Required
    flexible_days: int = 0,        # Optional with default
    cabin_class: str | None = None # Optional, can be None
):
    """Search for flights."""
    pass
```

## Multi-Agent Systems

### Agent Hierarchy
```python
from google.adk import Agent, SequentialAgent

researcher = Agent(
    name="researcher",
    model="gemini-2.0-flash",
    instruction="Research topics thoroughly.",
    tools=[search_tool],
    output_key="research_results"
)

writer = Agent(
    name="writer",
    model="gemini-2.0-flash",
    instruction="Write content based on research in state['research_results'].",
    output_key="final_content"
)

pipeline = SequentialAgent(
    name="content_pipeline",
    sub_agents=[researcher, writer]
)
```

### Communication Methods

1. **Shared Session State**: Agents read/write `context.state`
2. **Agent Transfer**: LLM calls `transfer_to_agent(agent_name='target')`
3. **AgentTool**: Wrap agent as callable tool for explicit invocation

## Workflow Agents

### SequentialAgent
```python
from google.adk import SequentialAgent

pipeline = SequentialAgent(
    name="pipeline",
    sub_agents=[agent1, agent2, agent3]  # Runs in order
)
```

### ParallelAgent
```python
from google.adk import ParallelAgent

parallel = ParallelAgent(
    name="parallel_tasks",
    sub_agents=[agent1, agent2, agent3]  # Runs concurrently
)
```

### LoopAgent
```python
from google.adk import LoopAgent

loop = LoopAgent(
    name="iterative_agent",
    sub_agents=[processor],
    max_iterations=5  # Or use escalate=True to exit
)
```

## Callbacks

Register callbacks for observability, guardrails, and control:

```python
def my_before_model_callback(callback_context, llm_request):
    """Called before each LLM call."""
    print(f"Calling model with: {llm_request}")
    return None  # Continue normally, or return LlmResponse to override

def my_after_tool_callback(callback_context, tool_response):
    """Called after each tool execution."""
    print(f"Tool returned: {tool_response}")
    return None

agent = Agent(
    name="my_agent",
    model="gemini-2.0-flash",
    instruction="...",
    before_model_callback=my_before_model_callback,
    after_tool_callback=my_after_tool_callback
)
```

**Callback Types:**
- `before_agent_callback` / `after_agent_callback`
- `before_model_callback` / `after_model_callback`
- `before_tool_callback` / `after_tool_callback`

## Sessions and State

```python
# State is scoped to the current session
context.state["user_preference"] = "dark_mode"
preference = context.state.get("user_preference")

# Memory spans multiple sessions (requires MemoryService)
```

## Running Agents

### CLI Commands
```bash
# Terminal interaction
adk run my_agent           # Python
npx adk run agent.ts       # TypeScript

# Web UI (browser-based)
adk web                    # Python - http://localhost:8000
npx adk web                # TypeScript

# API Server
adk api_server             # Exposes RESTful API
```

### Programmatic Execution
```python
from google.adk import Runner, InMemorySessionService

session_service = InMemorySessionService()
runner = Runner(
    agent=root_agent,
    session_service=session_service
)

# Create a session
session = session_service.create_session(
    app_name="my_app",
    user_id="user123"
)

# Run the agent
async for event in runner.run_async(
    session_id=session.id,
    user_message="What's the weather in New York?"
):
    print(event)
```

## Supported Models

- **Gemini**: gemini-2.0-flash, gemini-2.5-flash, gemini-2.5-pro
- **Claude**: Via Anthropic API or Vertex AI
- **Vertex AI**: Various models
- **Ollama**: Local models
- **vLLM**: Self-hosted models
- **LiteLLM**: Proxy to multiple providers

## Pre-Built Tools

### Gemini Tools
- Google Search
- Code Execution
- Computer Use

### Google Cloud Tools
- BigQuery, Bigtable, Spanner
- Vertex AI RAG Engine
- Vertex AI Search
- API Registry

### Third-Party Integrations
- GitHub, GitLab
- Stripe, Notion
- Linear, Atlassian

## Common Patterns

### Coordinator Pattern
Central agent routes to specialists:
```python
coordinator = Agent(
    name="coordinator",
    model="gemini-2.0-flash",
    instruction="""Route requests to the appropriate specialist:
    - Use 'researcher' for information gathering
    - Use 'writer' for content creation
    - Use 'analyst' for data analysis""",
    sub_agents=[researcher, writer, analyst]
)
```

### Generator-Critic Pattern
```python
generator = Agent(
    name="generator",
    output_key="draft"
)

critic = Agent(
    name="critic",
    instruction="Review the draft in state['draft'] and provide feedback."
)

pipeline = SequentialAgent(
    name="review_pipeline",
    sub_agents=[generator, critic]
)
```

## Instruction Writing Best Practices

1. **Be clear and specific** about the agent's role and capabilities
2. **Use markdown formatting** for structure
3. **Provide examples** of expected inputs and outputs
4. **Explain when and why** to use each tool
5. **Define constraints** and boundaries
6. **Include error handling guidance**

Example:
```python
instruction = """You are a customer service agent for TechCorp.

## Your Role
- Answer questions about our products
- Help troubleshoot common issues
- Escalate complex problems to human support

## Available Tools
- `search_knowledge_base`: Use for product information and FAQs
- `check_order_status`: Use when customer asks about their order
- `create_support_ticket`: Use for issues requiring human follow-up

## Guidelines
- Always greet customers warmly
- Ask clarifying questions when needed
- Never share internal system details
- If unsure, offer to connect with human support
"""
```
