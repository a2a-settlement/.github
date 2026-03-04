# A2A Settlement Exchange

**Escrow-based payments for the [Agent-to-Agent (A2A) protocol](https://github.com/google/A2A).**

When one agent does work for another, funds are held in escrow during execution — released on success, refunded on failure. Zero modifications to A2A core. Currency-agnostic. Security-first.

---

### What's new in v1.0.0

**Provenance attestation** — Requesters can now require providers to prove the origin of their deliverables. Three verification tiers (`self_declared`, `signed`, `verifiable`) let agents balance trust against cost. The mediator verifies provenance automatically during disputes, and the dashboard surfaces verification results for human review.

**Deliver step** — A new `deliver` operation sits between task completion and release. Providers submit their deliverable (with optional provenance metadata) to the exchange before the requester releases funds, giving both sides an auditable record of what was produced and where it came from.

| Feature | Where |
|---|---|
| Provenance verification (3-tier) | [a2a-settlement-mediator](https://github.com/a2a-settlement/a2a-settlement-mediator) |
| `deliver` API + `required_attestation_level` | [a2a-settlement](https://github.com/a2a-settlement/a2a-settlement) |
| `settlement:escrow:deliver` scope + wildcard path matching | [a2a-settlement-auth](https://github.com/a2a-settlement/a2a-settlement-auth) |
| Provenance & delivery UI in escrow detail | [a2a-settlement-dashboard](https://github.com/a2a-settlement/a2a-settlement-dashboard) |
| `settlement_deliver` tool + provenance on dispute resolution | [a2a-settlement-mcp](https://github.com/a2a-settlement/a2a-settlement-mcp) |
| `deliver()` / `deliver_escrow` tool | [adk-a2a-settlement](https://github.com/a2a-settlement/adk-a2a-settlement) |
| `deliver()` on `A2ASettlementClient` | [crewai-a2a-settlement](https://github.com/a2a-settlement/crewai-a2a-settlement) |
| `deliver_escrow_node` graph node | [langgraph-a2a-settlement](https://github.com/a2a-settlement/langgraph-a2a-settlement) |
| `required_attestation_level` per model | [litellm-a2a-settlement](https://github.com/a2a-settlement/litellm-a2a-settlement) |

---

### Core

| Repository | Description |
|---|---|
| [a2a-settlement](https://github.com/a2a-settlement/a2a-settlement) | Exchange API, Python & TypeScript SDKs, Security Shim (Economic Air Gap) |
| [a2a-settlement-auth](https://github.com/a2a-settlement/a2a-settlement-auth) | OAuth 2.0 settlement scopes — spending limits, counterparty policies, delegation chains |
| [a2a-settlement-mediator](https://github.com/a2a-settlement/a2a-settlement-mediator) | AI-powered dispute resolution with provenance verification and SEC 17a-4 WORM-compliant audit trail |
| [a2a-settlement-dashboard](https://github.com/a2a-settlement/a2a-settlement-dashboard) | Human oversight dashboard — monitor spending, audit transactions, inspect provenance, revoke authority |
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
   │    (attestation_level)   │                           │
   │                          │◄── accept_escrow ─────────┤
   │                          │                           │
   │                     funds held                       │
   │                          │                           │
   │                          │◄── deliver ───────────────┤
   │                          │    (content + provenance)  │
   │                          │                           │
   ├── release_escrow ───────►│                           │
   │                          ├── payout ────────────────►│
```

1. **Discover** — agents find each other via A2A
2. **Escrow** — the requester locks funds and optionally sets a required attestation level
3. **Execute** — the provider does the work
4. **Deliver** — the provider submits the deliverable with provenance metadata
5. **Settle** — funds release on success, refund on failure, or escalate to the mediator

### Provenance Tiers

| Tier | Name | Verification |
|---|---|---|
| 1 | `self_declared` | Plausibility checks — valid URIs, timestamps, source consistency |
| 2 | `signed` | Signature and hash validation with probabilistic spot-checks |
| 3 | `verifiable` | Active verification — re-fetches source URIs and confirms content hashes |

The mediator auto-upgrades to Tier 3 for high-value escrows. Provenance results feed into LLM-powered dispute evaluation and are stored in the WORM-compliant audit trail.

### Design Principles

- **Zero A2A modifications** — settlement is an extension, not a fork
- **Currency-agnostic** — hosted, self-hosted, or on-chain implementations behind one interface
- **Reputation-gated execution** — agents earn trust through settlement history
- **Provenance-aware** — deliverables carry attestations that are verified before funds move
- **Economic Air Gap** — the Security Shim prevents prompt-injection from triggering unauthorized payments
- **Human oversight** — the dashboard provides kill switches, spending controls, and provenance inspection

---

<sub>Built for a world where agents have wallets.</sub>
