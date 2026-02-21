# Claude Code Prompt Caching Optimization Guide

> 6 key strategies to improve caching efficiency by 10x

## Core Principle

**Prompt caching is prefix matching** — the order of your content determines cache hit rate. Put static content first, dynamic content last.

## The 6 Rules

### 1. Prefix Match is Everything

Static content should come first, dynamic content last. This maximizes shared prefix:

```
Layer 1: System prompt + tool definitions (always same)
Layer 2: Project context (e.g., Claude.md)
Layer 3: Session context
Layer 4: Current conversation messages
```

**Pitfalls to avoid:**
- Timestamps in system prompt
- Unstable tool ordering
- Minor tool parameter changes (breaks entire cache)

### 2. Use System Messages, Not System Prompts

When content changes (date, file updates, mode switches), DON'T edit the system prompt — this causes cache miss.

Instead, inject updates via **system messages / reminders** in the next turn to preserve the prefix.

```python
# Bad: edits system prompt
system_prompt = f"Today is {date}"  # ❌ cache miss every day

# Good: uses reminder
system_prompt = "Today is [DATE]"  # ✓ static
reminder = "Current date: 2026-02-20"  # ✓ injected separately
```

### 3. Don't Switch Models Mid-Session

Cache is model-specific. If you switch models, you lose all cached context — may end up more expensive.

**Better approach:** Use sub-agents. Let the large model generate a handoff to a smaller model for local tasks.

### 4. Don't Add/Remove Tools Mid-Session

Tool set is part of the cache prefix. Adding/removing tools = complete cache invalidation.

**Correct approach:** Keep tools stable. Use state-switching tools like `EnterPlanMode` / `ExitPlanMode` with system message constraints (e.g., "plan mode = read-only"). This preserves cache while allowing mode changes.

### 5. Too Many Tools? Use Lazy Loading

Don't remove tools. Keep stable lightweight stubs (same order, same names), use `ToolSearch` to discover and load full schemas when needed.

```python
# Stable stub
tools = [
  {"name": "Read", "description": "..."},
  {"name": "Edit", "description": "..."},
  # ... more stubs
]

# Load full schema on demand
def on_tool_call(tool_name):
    if tool_name not in loaded_tools:
        loaded_tools[tool_name] = fetch_full_schema(tool_name)
```

### 6. Compaction Must Be Cache-Safe

When summarizing/compressing context, don't open a separate request with different system prompt and no tools — this causes 0% cache hit, exploding costs.

**Correct approach:** Summary request uses the SAME system prompt, context, tool definitions, and history as parent session. Just append compaction instruction at the end — parent prefix continues to hit.

Also reserve compaction buffer space so you can append instructions when context is full.

## Monitor Cache Hit Rate

Treat cache hit rate as an availability metric. When it drops, treat it as an incident — even a few percent miss can significantly increase cost and latency.

## Quick Checklist

- [ ] Static content before dynamic content
- [ ] No timestamps in system prompt
- [ ] Stable tool order
- [ ] Use reminders for updates
- [ ] No model switching mid-session
- [ ] State switching via tools, not tool changes
- [ ] Lazy load tool schemas
- [ ] Compaction uses same prefix
- [ ] Monitor cache hit rate

---

**Source:** Based on @0xKingsKuan's tweet about Claude Code prompt caching best practices.
