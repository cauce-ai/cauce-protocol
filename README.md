# Cauce Protocol

Open protocol enabling AI agents to intelligently manage all your
communications in one place.

## Overview

Cauce is a unified protocol for personal communication arbitrage. It
provides a standardized way to:

- **Receive signals** from diverse sources (email, SMS, Slack, voice, etc.)
- **Route intelligently** through a pub/sub hub architecture
- **Enable AI agents** to prioritize, filter, summarize, and respond
- **Interoperate** with the broader AI ecosystem via A2A protocol support

Think of it as a **universal inbox protocol** - your AI agent subscribes to
all your communication channels through one interface.

## Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                       CAUCE ECOSYSTEM                           │
│                                                                 │
│  [Email] → [Adapter] ──┐                                        │
│  [SMS]   → [Adapter] ──┼──→ [Hub] ←──→ [Your AI Agent]          │
│  [Slack] → [Adapter] ──┘      ↑                                 │
│                               └────── [External A2A Agents]     │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

**Components:**

| Component   | Role                                                      |
|-------------|-----------------------------------------------------------|
| **Adapter** | Connects to a source, publishes signals, exposes capabilities |
| **Hub**     | Pub/sub broker, routes signals/actions, exposes MCP tools |
| **Agent**   | Subscribes to signals, invokes capabilities, decides actions |

## Key Features

- **Pub/Sub Architecture**: Everything is topics and subscriptions
- **Adapter Capabilities**: Adapters expose operations agents can invoke on-demand
- **Transport Agnostic**: WebSocket, SSE, polling, long-polling, webhooks
- **Privacy First**: TLS mandatory, end-to-end encryption optional
- **A2A Compatible**: External agents can participate as first-class citizens
- **Agent Agnostic**: Protocol defines communication, not AI logic

## Quick Example

**Inbound signal (email received):**
```json
{
  "id": "sig_1704288600_abc123",
  "topic": "signal.email.received",
  "source": { "type": "email", "adapter_id": "gmail_01" },
  "payload": {
    "raw": {
      "from": "alice@example.com",
      "subject": "Meeting tomorrow",
      "body_text": "Hi, can we meet at 3pm?"
    }
  }
}
```

**Outbound action (send reply):**
```json
{
  "id": "act_1704289000_xyz789",
  "topic": "action.email.send",
  "action": {
    "type": "send",
    "target": "email",
    "payload": {
      "to": ["alice@example.com"],
      "subject": "Re: Meeting tomorrow",
      "body_text": "3pm works for me!"
    }
  }
}
```

**Adapter capability (search emails):**

Adapters can expose capabilities that agents invoke on-demand via MCP tools:

```json
// Agent calls MCP tool: gmail_personal.search
{
  "name": "gmail_personal.search",
  "arguments": {
    "query": "from:alice@example.com meeting",
    "limit": 5
  }
}

// Adapter returns results
{
  "results": [
    { "id": "email_123", "subject": "Meeting tomorrow", "from": "alice@example.com" },
    { "id": "email_089", "subject": "Meeting notes", "from": "alice@example.com" }
  ],
  "total": 2
}
```

## Documentation

- [Full Protocol Specification](./cauce-protocol-spec.md)
- [JSON Schemas](./schemas/)

### Schemas

```
schemas/
├── signal.schema.json          # Core signal format
├── action.schema.json          # Outbound action format
├── capability.schema.json      # Adapter capability definition
├── jsonrpc.schema.json         # JSON-RPC message envelope
├── errors.schema.json          # Error codes and formats
├── methods/
│   ├── hello.schema.json       # Handshake (includes adapter_capabilities)
│   ├── subscribe.schema.json   # Subscription management
│   ├── unsubscribe.schema.json
│   ├── publish.schema.json     # Publishing messages
│   ├── ack.schema.json         # Acknowledgments
│   ├── subscription.schema.json # Subscription lifecycle
│   ├── schemas.schema.json     # Schema discovery
│   └── capability.schema.json  # Capability invocation
├── capabilities/               # Examples (Adapter-defined, not normative)
│   └── email.capabilities.json # Email adapter capabilities
└── payloads/                   # Examples (Hub-defined, not normative)
    ├── email.schema.json       # Email-specific payloads
    ├── sms.schema.json         # SMS/MMS payloads
    ├── slack.schema.json       # Slack payloads
    └── voice.schema.json       # Voice/audio payloads
```

## Roadmap

| Phase | Deliverable                  | Status  |
|-------|------------------------------|---------|
| 1     | Protocol Specification       | Draft   |
| 2     | JSON Schemas                 | Done    |
| 3     | Rust SDK                     | Planned |
| 4     | Python/TypeScript Bindings   | Planned |
| 5     | Reference Implementations    | Planned |

## Design Principles

1. **Hub is thin** - Pure message routing, no business logic
2. **Agents own intelligence** - Extraction, identity resolution, arbitrage
3. **Adapters are simple** - Just translate source ↔ Cauce format
4. **Free-form topics** - With recommended conventions for interoperability
5. **Subscriptions can require approval** - User controls who sees their data

## License

[Apache 2.0](./LICENSE)

## Contributing

This project is in early development. Contributions, feedback, and
discussions are welcome.
