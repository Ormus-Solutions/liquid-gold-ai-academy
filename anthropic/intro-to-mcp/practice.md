# Intro to MCP: Practice Questions

Self-made from the public docs and spec. Not the course quiz.

1. **What problem does MCP solve for an app developer?**
   Answer: You do not have to write and maintain tool schemas and integration code for every service; an MCP server exposes them in a standard way.

2. **Host, client, server: which one talks to the external API?**
   Answer: The server.

3. **What message format does MCP use?**
   Answer: JSON-RPC 2.0.

4. **Who controls tools, resources and prompts?**
   Answer: Model, application, user, in that order.

5. **You want a list of documents to power @-mention autocomplete in your UI. Tool or resource?**
   Answer: Resource. The app reads it directly; no model decision is needed.

6. **You want Claude to be able to edit a document. Which primitive?**
   Answer: Tool. It is an action the model decides to take.

7. **You want a "/format" command that gives Claude a well-crafted instruction. Which primitive?**
   Answer: Prompt. The user selects it.

8. **How does the Python SDK build a tool's input schema?**
   Answer: From the decorated function's type hints and docstring, with pydantic `Field` descriptions for arguments.

9. **Direct vs templated resource?**
   Answer: Direct has a fixed URI (`docs://documents`). Templated has parameters in the URI (`docs://documents/{doc_id}`) that map to function arguments.

10. **Why set a MIME type on a resource?**
    Answer: So the client knows whether to parse the content as JSON, text, or something else.

11. **What is the MCP Inspector for?**
    Answer: Testing and debugging a server in the browser: listing and calling tools, reading resources, rendering prompts, without an AI client.

12. **Which method lists a server's tools, and which runs one?**
    Answer: `tools/list` and `tools/call`.

13. **Walk the flow when Claude needs a tool.**
    Answer: Client fetches tool list, app sends tools plus query to Claude, Claude requests a tool use, client sends `tools/call`, server runs it and returns the result, result goes back to Claude, Claude answers.

14. **Is MCP tied to one transport?**
    Answer: No. It is transport agnostic. stdio is common locally; Streamable HTTP for remote servers.
