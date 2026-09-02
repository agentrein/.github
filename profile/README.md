<div align="center">

<img src="https://www.agentrein.com/AgentRein.png" width="560" alt="AgentRein"/>

# Wrap your AI agents. Undo anything.

### The rollback layer for AI agents.

Protect production AI agents with automatic rollback, approval gates, audit trails, and intent verification.

<br>

[![Docs](https://img.shields.io/badge/Docs-agentrein.com-45bb00?style=for-the-badge)](https://agentrein.com/docs)
[![Website](https://img.shields.io/badge/Website-agentrein.com-111827?style=for-the-badge)](https://agentrein.com)
[![npm](https://img.shields.io/npm/v/agentrein?style=for-the-badge&logo=npm)](https://www.npmjs.com/package/agentrein)
[![Discord](https://img.shields.io/badge/Discord-AgentRein-5865F2?style=for-the-badge&logo=discord)](https://discord.gg/EWkfguCV)

</div>

---

## Install

```bash
npm install agentrein
```

---

## Wrap

```ts
const session = await agentrein.newSession({
  agentId: "billing-agent",
  intent: "Create customer invoice"
});

const stripe = agentrein.wrap(stripeClient, session);
```

---

## Run

```ts
await stripe.invoices.create({
  customer,
  amount
});
```

That's it.

---

# Why AgentRein?

AI agents already know **how to act**.

Production systems don't know **how to recover**.

One incorrect action can:

- Generate the wrong invoice
- Email the wrong customer
- Modify production data
- Create the wrong GitHub issue
- Update the wrong Notion page

AgentRein makes production actions:

- Observable
- Approvable
- Reversible

---

# Features

| | |
|:-|:-|
| ↶ **Automatic Rollback** | Reverse supported production actions automatically. |
| ✓ **Approval Gates** | Require human approval before risky operations. |
| ◎ **Audit Trail** | Record every action, decision, input, and execution. |
| ◇ **Intent Verification** | Detect goal drift before agents reach production. |

---

# Example

```ts
const session = await agentrein.newSession({
  intent: "Onboard enterprise customer"
});

const stripe = agentrein.wrap(stripeClient, session);

await stripe.customers.create(...);
await stripe.invoices.create(...);
```

Automatically:

- Records execution metadata
- Captures rollback state
- Applies approval policies
- Executes rollback when requested

---

# Supported Integrations

| | | |
|:-|:-|:-|
| Stripe | Slack | GitHub |
| Notion | HubSpot | Salesforce |
| Gmail | Google Drive | Google Sheets |

More integrations are available through open-source connectors.

---

# Ecosystem

| Repository | Description |
|------------|-------------|
| **sdk** | TypeScript SDK |
| **agentrein-connectors** | Open-source rollback connectors |
| **docs** | Documentation |
| **examples** *(coming soon)* | End-to-end examples |

---

# Documentation

- 📖 **Docs** — https://agentrein.com/docs
- ⚡ **Quick Start** — https://agentrein.com/docs/introduction
- 🔌 **Connectors** — https://github.com/agentrein/agentrein-connectors
- 💬 **Discord** — https://discord.gg/EWkfguCV

---

# Contributing

AgentRein is built in the open.

We welcome:

- Connectors
- Examples
- Bug fixes
- Documentation improvements
- Feature proposals

See the contributing guide to get started.

---

<div align="center">

## Let them act.

# Sleep at night.

</div>
