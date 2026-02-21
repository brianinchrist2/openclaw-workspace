# How to Deploy Multiple AI Agents with OpenClaw

This guide explains how to deploy multiple AI agents that can work together, based on the multi-agent team setup described in the OpenClaw community.

## Overview

You can run multiple specialized AI agents that work together in a team. Each agent has a specific role and they can communicate through a single chat interface (like Telegram or Feishu).

## Example Team Structure

Based on the popular setup, here's a typical multi-agent team:

| Agent | Role | Responsibilities |
|-------|------|------------------|
| 大总管 (Manager) | Team Lead | Coordinate tasks, route requests to other agents |
| 开发助理 (Dev Assistant) | Developer | Code review, bug fixes, programming tasks |
| 运营增长 (Operations) | Growth | Marketing, analytics, user engagement |
| 内容助理 (Content) | Content | Writing, social media, articles |
| 法务助理 (Legal) | Legal | Contract review, compliance checks |
| 财务助理 (Finance) | Finance | Budget tracking, financial analysis |

## Deployment Methods

### Method 1: Multi-Agent via Subagents

Use OpenClaw's built-in subagent feature to spawn multiple agents:

```bash
# In your agent session, spawn subagents for different roles
sessions_spawn --agentId dev-assistant --task "You are a coding assistant..."
sessions_spawn --agentId content-assistant --task "You are a content writer..."
```

### Method 2: Multiple Gateway Instances

Run multiple OpenClaw Gateway processes with different configs:

```bash
# Start first agent on port 18789
openclaw gateway --port 18789 --agent-name "dev-assistant"

# Start second agent on port 18790
openclaw gateway --port 18790 --agent-name "content-assistant"
```

### Method 3: Telegram Group with Multiple Bots

This is the most popular method described in the article:

1. **Create a Telegram group**
2. **Add multiple bots** to the group
3. **Configure each bot** with a different persona/role
4. **Use @mentions** to call specific agents

#### Step-by-Step Setup:

**Step 1: Create Bot Tokens**

Create multiple Telegram bots via @BotFather:
- Get tokens for each agent (dev-bot, content-bot, legal-bot, etc.)

**Step 2: Configure OpenClaw**

```bash
# Configure each bot with a different agent
openclaw configure --channel telegram --bot-token "YOUR_TOKEN_1"
openclaw configure --channel telegram --bot-token "YOUR_TOKEN_2"
# ... repeat for each bot
```

**Step 3: Set Agent Personas**

In your workspace, create persona files for each agent:

```
workspace/
├── agents/
│   ├── manager.md      # 大总管 prompt
│   ├── dev.md          # 开发助理 prompt
│   ├── content.md      # 内容助理 prompt
│   ├── legal.md        # 法务助理 prompt
│   └── finance.md      # 财务助理 prompt
```

**Step 4: Start All Agents**

```bash
# Start all agent instances
openclaw gateway --port 18789 &
openclaw gateway --port 18790 &
# ... etc
```

**Step 5: Add Bots to Telegram Group**

Add all bots to one Telegram group. The manager bot can handle routing.

### Method 4: Feishu (飞书) Multi-Agent

Similar to Telegram but using Feishu:

```bash
# Configure Feishu channel
openclaw configure --channel feishu --app-id "YOUR_APP_ID" --app-secret "YOUR_SECRET"
```

## Agent Persona Template

Here's a template for creating agent personas:

```markdown
# Agent Persona: [Role Name]

## Identity
- Name: [Chinese Name]
- Role: [English Role]
- Description: [Brief description]

## Core Responsibilities
- [Responsibility 1]
- [Responsibility 2]
- [Responsibility 3]

## Skills
- [Skill 1]
- [Skill 2]

## Communication Style
- Be professional but friendly
- Respond in [language]
- Use [tone] tone

## Constraints
- [Constraint 1]
- [Constraint 2]

## Examples
- When user says: "[Example request 1]"
  Respond with: "[Example response 1]"
```

## Multi-Agent Communication Pattern

### Direct @Mention

```
@开发助理 帮我看看这段代码有没有 bug
@内容助理 帮我写个公众号开头
@法务助理 这个合同的竞业条款有没有坑
```

### Manager Routing

The manager agent (大管家) can:
1. Receive user requests
2. Analyze the request
3. Route to the appropriate specialist agent
4. Synthesize responses from multiple agents

## Best Practices

### 1. Clear Role Definition
Define each agent's responsibilities clearly to avoid confusion.

### 2. Use Consistent Naming
Use consistent naming conventions across all agents for easy identification.

### 3. Manager Agent Pattern
Have a coordinator agent that routes requests to specialists.

### 4. Context Sharing
Enable context sharing between agents when needed for complex tasks.

### 5. Monitoring
Monitor agent performance and adjust roles as needed.

## Scaling Up

To scale from 6 to 12 agents:

1. **Add more channels**: Run agents on both Telegram AND Feishu
2. **Add more specialties**: Add QA, HR, Customer Support agents
3. **Use different models**: Assign faster models to simple tasks, smarter models to complex ones
4. **Schedule agents**: Use cron jobs to start/stop agents based on workload

## Troubleshooting

### Issue: Agents not responding
- Check if Gateway is running
- Verify bot tokens are valid
- Check network connectivity

### Issue: Agents not following roles
- Review and update persona definitions
- Check system prompt configuration

### Issue: Too many agents slowing down
- Use prompt caching (review Claude Code caching guide)
- Optimize agent configurations
- Use lighter models for simple tasks

## Related Resources

- OpenClaw Documentation: https://docs.openclaw.ai
- Awesome OpenClaw Use Cases: https://github.com/hesamsheikh/awesome-openclaw-usecases
- Multi-Agent Team Pattern: See "Multi-Agent Specialized Team" use case

## Credits

This guide is based on the article by @jungeAGI (俊哥AI) about running 12 AI employees on Feishu and Telegram.
