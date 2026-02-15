---
title: "SwarmCore: Multi-Agent Orchestration for Python"
description: Compose AI agents into sequential and parallel flows with automatic context sharing.
pubDate: 2026-02-12
featured: true
---

SwarmCore is a Python library for composing AI agents into workflows. You define agents, wire them together with `>>` (sequential) and `|` (parallel), and run them.

## Why

Most agent frameworks are heavy. They want you to buy into a whole ecosystem — custom runtimes, proprietary tool formats, YAML configs. I wanted something that feels like writing normal Python.

## How it works

Agents are functions with instructions and a model. You compose them with operators:

```python
researcher() >> writer()                            # sequential
(analyst() | writer()) >> editor()                  # parallel then sequential
researcher() >> (analyst() | writer()) >> editor()  # mixed
```

Each agent in a flow receives context from prior steps automatically. The previous step's full output is passed through, and earlier steps are summarized to keep context windows manageable. Agents can call `expand_context` to retrieve any prior agent's full output on demand.

## Built-in agent factories

SwarmCore ships with pre-built factories for common roles — `researcher`, `analyst`, `writer`, `editor`, `summarizer` — each with sensible defaults. Zero config for prototyping, full override for production.

```python
from swarmcore import researcher, analyst, writer

flow = researcher() >> analyst() >> writer()
```

## Any model

It works with any LiteLLM-compatible provider. Anthropic, OpenAI, Gemini, Groq, Ollama — whatever you have an API key for. Gemini's free tier works out of the box.

## Tools

Tools are plain Python functions. Type hints and docstrings become the tool schema automatically.

```python
def search_web(query: str) -> str:
    """Search the web for information."""
    return results

agent = Agent(name="researcher", instructions="...", tools=[search_web])
```

The repo is at [github.com/MatchaOnMuffins/swarmcore](https://github.com/MatchaOnMuffins/swarmcore) and it's on [PyPI](https://pypi.org/project/swarmcore/).
