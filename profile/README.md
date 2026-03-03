# A2A Settlement Exchange

**Escrow-based payments for the [Agent-to-Agent (A2A) protocol](https://github.com/google/A2A).**

When one agent does work for another, funds are held in escrow during execution — released on success, refunded on failure. Zero modifications to A2A core. Currency-agnostic. Security-first.

---

### Core

| Repository | Description |
|---|---|
| [a2a-settlement](https://github.com/a2a-settlement/a2a-settlement) | Exchange API, Python & TypeScript SDKs, Security Shim (Economic Air Gap) |
| [a2a-settlement-auth](https://github.com/a2a-settlement/a2a-settlement-auth) | OAuth 2.0 settlement scopes — spending limits, counterparty policies, delegation chains |
| [a2a-settlement-mediator](https://github.com/a2a-settlement/a2a-settlement-mediator) | AI-powered dispute resolution with SEC 17a-4 WORM-compliant audit trail |
| [a2a-settlement-dashboard](https://github.com/a2a-settlement/a2a-settlement-dashboard) | Human oversight dashboard — monitor spending, audit transactions, revoke authority |
| [a2a-settlement-mcp](https://github.com/a2a-settlement/a2a-settlement-mcp) | MCP server exposing settlement operations as tools for any MCP client |

### Framework Integrations

| Repository | Framework |
|---|---|
| [adk-a2a-settlement](https://github.com/a2a-settlement/adk-a2a-settlement) | Google Agent Development Kit (ADK) |
| [crewai-a2a-settlement](https://github.com/a2a-settlement/crewai-a2a-settlement) | CrewAI |
| [langgraph-a2a-settlement](https://github.com/a2a-settlement/langgraph-a2a-settlement) | LangGraph |
| [litellm-a2a-settlement](https://github.com/a2a-settlement/litellm-a2a-settlement) | LiteLLM |

---

### How It Works

```
Agent A                    Exchange                    Agent B
   │                          │                           │
   ├── create_escrow ────────►│                           │
   │                          │◄── accept_escrow ─────────┤
   │                          │                           │
   │                     funds held                       │
   │                          │                           │
   │                          │◄── work complete ─────────┤
   ├── release_escrow ───────►│                           │
   │                          ├── payout ────────────────►│
```

1. **Discover** — agents find each other via A2A  
2. **Escrow** — the requester locks funds in the exchange  
3. **Execute** — the provider does the work  
4. **Settle** — funds release on success, refund on failure, or escalate to the mediator  

### Design Principles

- **Zero A2A modifications** — settlement is an extension, not a fork
- **Currency-agnostic** — hosted, self-hosted, or on-chain implementations behind one interface
- **Reputation-gated execution** — agents earn trust through settlement history
- **Economic Air Gap** — the Security Shim prevents prompt-injection from triggering unauthorized payments
- **Human oversight** — the dashboard provides kill switches and spending controls

---

<sub>Built for a world where agents have wallets.</sub>
