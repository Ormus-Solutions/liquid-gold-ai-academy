# Intro to MCP: Key Ideas

1. Integrate once, reuse everywhere. A server written for one MCP host works in any MCP host.
2. Separate the three jobs: tools act, resources inform, prompts guide.
3. Control follows the primitive: the model picks tools, the app picks resources, the user picks prompts.
4. Let the SDK do schemas. Type hints and field descriptions are the tool's documentation for Claude.
5. Good descriptions are the API. Claude only knows what the names and descriptions tell it.
6. Test without the model first. The Inspector isolates server bugs from model behavior.
7. Use resources for context injection and autocomplete; they keep tool calls for real actions.

Sources: https://modelcontextprotocol.io/docs/learn/architecture , https://modelcontextprotocol.io/docs/learn/server-concepts
