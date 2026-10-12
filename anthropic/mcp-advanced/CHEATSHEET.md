# MCP: Advanced Topics: Cheat Sheet

Course: https://anthropic.skilljar.com/model-context-protocol-advanced-topics (ORM-295)
Sources: [Spec: lifecycle](https://modelcontextprotocol.io/specification/latest/basic/lifecycle), [Spec: transports](https://modelcontextprotocol.io/specification/latest/basic/transports), [Spec: sampling](https://modelcontextprotocol.io/specification/latest/client/sampling), [Spec: roots](https://modelcontextprotocol.io/specification/latest/client/roots), [Spec: progress](https://modelcontextprotocol.io/specification/latest/basic/utilities/progress), [Spec: logging](https://modelcontextprotocol.io/specification/latest/server/utilities/logging), [Python SDK](https://github.com/modelcontextprotocol/python-sdk)

Written from public docs and the spec in our own words. Not a quiz key.

## Client features a server can use
- **Sampling** (`sampling/createMessage`): the server asks the client to run an LLM call. The client owns the model, API key, cost and user approval. Server stays model-free and cheap; good for public servers.
- **Roots** (`roots/list`): the client tells the server which directories or URIs it may work in. A security and scoping boundary, plus a way to find files without full paths. Client sends `notifications/roots/list_changed` when they change. Note: the server is expected to respect roots; enforcement is in the server's code.
- **Elicitation**: newer feature where the server asks the user for input through the client.

## Notifications for long work
- **Logging**: server sends `notifications/message` with a level. In the Python SDK, via the `Context` object: `await ctx.info(...)`, `ctx.debug`, `ctx.warning`, `ctx.error`.
- **Progress**: client attaches a `progressToken` to a request; server sends `notifications/progress` with `progress` and optional `total`. SDK: `await ctx.report_progress(done, total)`. Client handles them with logging and progress callbacks.

## JSON-RPC message types
| Type | Has id | Expects reply | Example |
|---|---|---|---|
| Request | yes | yes | `tools/call`, `sampling/createMessage` |
| Result / Error | same id | no | response to a request |
| Notification | no | no | `notifications/progress`, `notifications/initialized` |

Both sides can send requests: client to server (tools/call) and server to client (sampling, roots/list, ping).

## Lifecycle handshake
1. Client sends `initialize` request (protocol version, capabilities, client info).
2. Server returns `initialize` result (its capabilities).
3. Client sends `notifications/initialized`.
Only then do normal requests flow.

## Transports
- **stdio**: client launches the server as a subprocess. Messages go over stdin and stdout, newline delimited. Stderr is for logs. Both directions can initiate anytime. Local only, simplest.
- **Streamable HTTP**: one MCP endpoint. Client sends each message as an HTTP POST. Server replies with plain JSON or opens an SSE stream on that response to send progress, logs and requests tied to it. Client may open a GET SSE stream so the server can send messages not tied to any request. Server can assign a session with the `Mcp-Session-Id` header at initialize; client sends it on later requests.
- Replaces the older HTTP+SSE transport (two endpoints).

## Scaling flags (Python SDK)
- `stateless_http=True`: no session ID, no memory between requests. Any instance behind a load balancer can serve any request (horizontal scaling). Cost: no server-initiated requests, so no sampling, no roots requests, no standalone GET stream for notifications.
- `json_response=True`: POST responses are plain JSON, not SSE streams. Cost: no streamed progress or log messages during a call; you only get the final result.

## 10 facts most likely tested
1. Sampling moves LLM cost and complexity from the server to the client.
2. Notifications have no id and get no response.
3. Requests always get a result or error with the matching id.
4. Handshake: initialize request, initialize result, initialized notification.
5. stdio runs the server as a child process over stdin and stdout.
6. Streamable HTTP uses POST for client messages and SSE for server-to-client streaming.
7. A GET to the endpoint opens a stream for server-initiated messages.
8. Sessions are tracked with the `Mcp-Session-Id` header.
9. Stateless HTTP enables load balancing but drops server-to-client requests.
10. JSON response mode drops streaming of progress and logs.

## Common gotchas
- Thinking roots are enforced by the protocol; the server code must check paths.
- Printing debug text to stdout in a stdio server breaks the protocol stream; log to stderr.
- Turning on stateless HTTP and wondering why sampling fails.
- Expecting progress updates with `json_response=True`.
- Sending requests before the initialized notification.

## Transport choice
Local tool for one user: stdio. Remote, needs sampling or streaming: Streamable HTTP with sessions. Remote, high scale, simple tools: stateless HTTP, optionally JSON responses.
