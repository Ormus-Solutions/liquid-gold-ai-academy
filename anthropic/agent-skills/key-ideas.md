# Agent Skills: Key Ideas

1. Write it once. Any instruction you paste twice is a skill candidate.
2. The description is the router. Say what the skill does and the phrases a user would say when they need it.
3. Load in layers. Metadata is cheap and always present; the body loads on use; heavy reference and scripts load only when touched.
4. Code beats prose for deterministic steps. Bundle a script and let Claude run it instead of reading it.
5. Pick the right tool for the job: CLAUDE.md for facts, skills for procedures, hooks for guarantees, subagents for isolation, MCP for external access.
6. Scope decides reach: personal for you, project for the repo, plugin for many repos, managed settings for the org.
7. Control who can fire it. Side effects mean manual only.
8. Debug from the outside in: is it listed, does the description match, does the YAML parse.

Sources: https://code.claude.com/docs/en/skills , https://docs.claude.com/en/docs/agents-and-tools/agent-skills/best-practices
