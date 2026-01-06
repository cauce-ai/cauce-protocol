# Example Payload Schemas

**These schemas are examples, not part of the official Cauce Protocol.**

Each Hub defines its own payload schemas based on the communication sources it supports. The schemas in this directory demonstrate common patterns for typical sources (email, SMS, Slack, voice) but are not normative.

## Hub-Defined Schemas

Hubs expose their supported schemas via:
- `cauce.schemas.list` - List all supported payload schemas
- `cauce.schemas.get` - Retrieve a specific schema definition
- `cauce://schemas` - MCP resource for schema discovery

## Using These Examples

These examples can serve as a starting point for Hub implementations. When implementing a Hub, you may:
- Use these schemas as-is
- Extend them with additional fields
- Create entirely different schemas for your use case

The only protocol requirement is that payloads SHOULD include `raw`, `content_type`, and `size_bytes` fields (see Section 5.3.1 of the spec).
