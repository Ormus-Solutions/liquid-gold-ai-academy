# MCP Advanced: Practice Questions

Self-made from the spec and SDK docs. Not the course quiz.

1. **What does sampling let a server do, and who pays for the model call?**
   Answer: Ask the client to run an LLM completion. The client does, so cost, keys and model choice stay with the client.

2. **Why is sampling attractive for a publicly hosted server?**
   Answer: The server needs no API key and carries no token cost; users bring their own model through their client.

3. **What does a client attach to a request to receive progress updates?**
   Answer: A `progressToken`. The server then sends `notifications/progress` referencing it.

4. **In the Python SDK, how does a tool send a log line and a progress update?**
   Answer: Through the `Context` object: `await ctx.info("...")` and `await ctx.report_progress(done, total)`.

5. **What are roots?**
   Answer: Directories or URIs the client exposes to the server, defining where the server may operate. The server lists them with `roots/list`.

6. **Does the protocol itself stop a server from reading outside its roots?**
   Answer: No. Roots communicate the boundary; the server implementation must enforce it.

7. **Request vs notification?**
   Answer: A request has an id and must get a result or error. A notification has no id and gets no reply.

8. **Order the initialization messages.**
   Answer: Client `initialize` request, server `initialize` result, client `notifications/initialized`.

9. **How does the stdio transport work?**
   Answer: The client starts the server as a subprocess and exchanges newline-delimited JSON-RPC over stdin and stdout. Both sides can send at any time.

10. **Why should a stdio server never print debug text to stdout?**
    Answer: Stdout carries protocol messages; stray text corrupts the stream. Use stderr.

11. **In Streamable HTTP, how does the server push progress during a tool call?**
    Answer: It answers the POST with an SSE stream and sends notifications on it before the final result.

12. **What is the GET request to the MCP endpoint for?**
    Answer: Opening a standalone SSE stream so the server can send requests and notifications not tied to a client request.

13. **How is a session identified in Streamable HTTP?**
    Answer: The server returns an `Mcp-Session-Id` header at initialization, and the client includes it on every later request.

14. **What do you gain and lose with `stateless_http=True`?**
    Answer: Gain horizontal scaling behind a load balancer. Lose sessions and server-initiated requests such as sampling and roots listing.

15. **What do you lose with `json_response=True`?**
    Answer: Streaming within a response, so no live progress or log messages; only the final JSON result.
