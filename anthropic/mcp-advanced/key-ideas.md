# MCP Advanced: Key Ideas

1. The client owns the model. Sampling lets servers borrow intelligence without paying for it or holding keys.
2. Long tasks should talk. Progress and log notifications turn a silent wait into visible work.
3. Roots are a contract about where a server may work. Honor them in code.
4. Know the three message shapes: request expects a reply, result answers by id, notification is fire and forget.
5. Nothing happens before the handshake completes.
6. Transport decides capability. stdio and stateful HTTP are fully bidirectional; stateless and JSON-only modes trade features for scale.
7. Pick the simplest transport that supports the features you actually need.

Sources: https://modelcontextprotocol.io/specification/latest
