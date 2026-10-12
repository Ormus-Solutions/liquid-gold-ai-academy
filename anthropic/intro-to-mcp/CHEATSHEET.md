# Introduction to Model Context Protocol: Cheat Sheet

Course: https://anthropic.skilljar.com/introduction-to-model-context-protocol (ORM-296)
Sources: [MCP architecture](https://modelcontextprotocol.io/docs/learn/architecture), [MCP spec](https://modelcontextprotocol.io/specification/latest), [Server concepts](https://modelcontextprotocol.io/docs/learn/server-concepts), [Python SDK](https://github.com/modelcontextprotocol/python-sdk), [MCP Inspector](https://modelcontextprotocol.io/docs/tools/inspector)

Written from public docs in our own words. Not a quiz key.

## Core concepts
- **MCP**: an open protocol that standardizes how AI apps get tools and context from external systems. Instead of writing and maintaining every tool schema and integration yourself, you connect to an MCP server that already exposes them.
- **Host**: the AI app (Claude Desktop, Claude Code, your app).
- **Client**: lives in the host, holds one connection to one server.
- **Server**: exposes capabilities (tools, resources, prompts) for a service such as GitHub or a document store.
- **Wire format**: JSON-RPC 2.0 messages. Transport agnostic (stdio locally, Streamable HTTP remotely).

## The three primitives (know who controls each)
| Primitive | Controlled by | Purpose | Methods |
|---|---|---|---|
| Tools | Model | Actions Claude decides to call | `tools/list`, `tools/call` |
| Resources | Application | Read-only data the app pulls into context | `resources/list`, `resources/read`, `resources/templates/list` |
| Prompts | User | Pre-built instruction templates the user picks (often slash commands) | `prompts/list`, `prompts/get` |

## Request flow
User asks -> host asks client for the server's tool list -> tools sent to Claude with the query -> Claude returns a tool use -> client sends `tools/call` to server -> server calls the external API -> result goes back to Claude -> Claude answers.

## Python SDK essentials (FastMCP)
- `mcp = FastMCP("name")`.
- `@mcp.tool()` on a typed function: the SDK builds the JSON schema from type hints and docstring. Use pydantic `Field(description=...)` for argument descriptions.
- `@mcp.resource("docs://documents")` for a direct resource with a fixed URI.
- `@mcp.resource("docs://documents/{doc_id}")` for a templated resource; URI parameters become function arguments.
- Resources declare a `mime_type` (for example `application/json` or `text/plain`) so the client knows how to parse.
- `@mcp.prompt()` returns a list of messages.
- Client side: `ClientSession` with `initialize()`, `list_tools()`, `call_tool()`, `read_resource()`, `list_prompts()`, `get_prompt()`.

## MCP Inspector
Browser UI for testing a server without an AI client. With the Python SDK: `mcp dev server.py`, or `npx @modelcontextprotocol/inspector`. Lets you list and call tools, read resources, and render prompts.

## 10 facts most likely tested
1. MCP shifts tool definition and execution work from your app to MCP servers.
2. Tools are model-controlled, resources app-controlled, prompts user-controlled.
3. Messages are JSON-RPC 2.0.
4. Each client keeps a one-to-one connection with a server.
5. The SDK generates tool schemas from decorators and type hints, so you do not hand-write JSON schema.
6. Resources are for reading data; tools are for doing things.
7. Templated resources use URI templates with `{param}` placeholders.
8. MIME type tells the client how to decode resource content.
9. The Inspector is the fastest way to debug a server.
10. MCP is transport agnostic; stdio for local, HTTP for remote.

## Common gotchas
- Using a tool where a resource fits (e.g. fetching a doc for @-mention autocomplete is a resource).
- Forgetting argument descriptions, which makes Claude call tools poorly.
- Mixing up who controls prompts (the user, not the model).
- Returning JSON from a resource but not marking it `application/json`.
