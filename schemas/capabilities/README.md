# Example Adapter Capabilities

**These capability definitions are examples, not part of the official Cauce
Protocol.**

Each adapter defines its own capabilities based on the operations it can
perform. The files in this directory demonstrate common patterns for typical
adapters but are not normative.

## Adapter-Defined Capabilities

Adapters register their capabilities during the `cauce.hello` handshake using
the `adapter_capabilities` field. Capabilities are:

- **Static**: Declared once at connection time
- **Namespaced**: Exposed to agents as `{adapter_name}.{capability_name}`
- **Invocable**: Called via MCP tools or `cauce.capability.invoke`

## Available Examples

| File | Description |
|------|-------------|
| `email.capabilities.json` | Email adapter: folders, search, read/unread, labels, attachments |

## Using These Examples

When implementing an adapter, you may:

- Use these capabilities as-is
- Extend them with additional operations
- Create entirely different capabilities for your use case

## Capability Schema

Each capability must include:

```json
{
  "name": "capability_name",
  "description": "What this capability does",
  "input_schema": { "type": "object", "properties": {...} },
  "output_schema": { "type": "object", "properties": {...} },
  "idempotent": true,
  "readonly": true,
  "timeout_hint_ms": 5000
}
```

See `capability.schema.json` in the parent directory for the full schema.
