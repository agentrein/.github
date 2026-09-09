# AgentRein

AgentRein provides an execution safety layer for AI agents. It sits between AI frameworks and external third-party APIs to handle automatic state rollbacks, human approval queues, intent policy validation, and audit logging.

When an AI agent makes bad tool calls or encounters errors mid-execution, AgentRein reverts the state changes made across integrated services without losing context or corrupting external data.

[![Docs](https://img.shields.io/badge/Docs-agentrein.com-blue?logo=mintlify&logoColor=white)](https://agentrein.com/docs)
[![npm](https://img.shields.io/npm/v/agentrein?color=CB3837&logo=npm)](https://www.npmjs.com/package/agentrein)
[![PyPI](https://img.shields.io/pypi/v/agentrein?color=3775A9&logo=pypi&logoColor=white)](https://pypi.org/project/agentrein/)
[![License](https://img.shields.io/badge/License-MIT-2ea44f)](https://opensource.org/licenses/MIT)

---

## How It Works

AgentRein intercepts tool calls made by agents running on frameworks like LangChain, LangGraph, CrewAI, or custom Python and Node.js setups.

    +----------------+      Tool Call      +-----------------------+      Validated Call      +----------------+
    |    AI Agent    | ------------------> | AgentRein Safety Core | -----------------------> | External APIs  |
    | (LangGraph/etc)| <------------------ |  - Policy Verification| <----------------------- | (Slack, Stripe,|
    +----------------+     Execution       |  - Snapshot Engine    |        API Response      |  Notion, etc.) |
                           Response        |  - Compensation Stack |                          +----------------+
                                           +-----------------------+

Every incoming API action gets categorized into safety groups:

* Group 1: Actions with direct, simple inverse API calls (like create and delete).
* Group 2: Actions requiring state snapshots before execution so previous state can be restored.
* Group 3: Symmetric toggle actions (like archive and restore or attach and detach).
* Group 4: High-risk or non-reversible actions (like permanent deletions or financial charges) that halt execution for human approval before running.

If an execution fails or an agent strays off-track, calling a session rollback triggers the compensation engine to execute exact inverse operations in reverse order.

---

## Main Repositories

* [backend](https://github.com/agentrein/backend): The core execution engine, compensation stack, Prisma schema, and WebSocket server.
* [agentrein-connectors](https://github.com/agentrein/agentrein-connectors): Built-in connectors for services like Salesforce, Stripe, Notion, Slack, GitHub, Google Drive, Google Sheets, Gmail, and HubSpot.
* [sdk](https://github.com/agentrein/sdk): Official TypeScript/JavaScript client library.
* [agentrein-python](https://github.com/agentrein/agentrein-python): Official Python SDK for agent integration.
* [types-package](https://github.com/agentrein/types-package): Shared TypeScript definitions and contract interfaces.
* [website](https://github.com/agentrein/website): Next.js web application for managing sessions, approval queues, and team credentials.
* [docs](https://github.com/agentrein/docs): Technical documentation source files.

---

## Quick Example

### TypeScript / Node.js

Installation:

    npm install agentrein

Usage:

    import { AgentReinClient } from 'agentrein';
    
    const client = new AgentReinClient({
      apiKey: process.env.AGENTREIN_API_KEY!,
    });
    
    const session = await client.createSession({
      agentId: 'support-agent-01',
    });
    
    const result = await session.executeAction({
      connector: 'slack',
      action: 'channels.invite',
      payload: { channelId: 'C1234567', userIds: ['U9876543'] },
    });
    
    if (!result.success) {
      await session.rollback();
    }

### Python

Installation:

    pip install agentrein

Usage:

    from agentrein import AgentReinClient
    import os

    client = AgentReinClient(api_key=os.environ.get("AGENTREIN_API_KEY"))

    session = client.create_session(agent_id="support-agent-01")

    result = session.execute_action(
        connector="slack",
        action="channels.invite",
        payload={"channel_id": "C1234567", "user_ids": ["U9876543"]}
    )

    if not result.get("success"):
        session.rollback()

---

## Resources & Community

* Documentation: [agentrein.com/docs](https://agentrein.com/docs)
* Website: [agentrein.com](https://agentrein.com)
* Bug Tracker: Please open issues in the specific repository where the bug occurs.
