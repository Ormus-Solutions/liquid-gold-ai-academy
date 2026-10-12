# Subagents: Practice Questions

Self-made from the public docs. Not the course quiz.

1. **Main benefit of a subagent for long sessions?**
   Answer: It works in a separate context window and returns only a summary, so the main context does not fill with search results or logs.

2. **Does a subagent see the earlier conversation?**
   Answer: No. It starts fresh with its system prompt and the delegation message. (A fork is the exception and inherits history.)

3. **Which two frontmatter fields are required?**
   Answer: `name` and `description`.

4. **How do you make a subagent read-only?**
   Answer: Set `tools` to read tools only (for example Read, Grep, Glob) or use `disallowedTools: Write, Edit`.

5. **What happens if `tools` is omitted?**
   Answer: The subagent inherits all tools available to subagents.

6. **Where do you put a subagent your whole team should share for one repo?**
   Answer: `.claude/agents/` in the repo, committed.

7. **Project and user subagent share a name. Which wins?**
   Answer: The project one (higher priority than user).

8. **How does Claude decide to delegate automatically?**
   Answer: It matches the task to subagent descriptions. Phrases like "use proactively" encourage delegation.

9. **Name the built-in read-only search subagent.**
   Answer: Explore.

10. **Why define a structured output format in the system prompt?**
    Answer: It makes the returned summary predictable and compact, so the main agent can act on it without re-reading raw output.

11. **Why ask subagents to report obstacles?**
    Answer: So they stop and explain instead of looping or failing silently, and the main agent can decide what to do next.

12. **Good or bad use: a two-line typo fix?**
    Answer: Bad. Startup and context gathering cost more than the task. Do it in the main conversation.

13. **Good or bad use: run the full test suite and report only failures?**
    Answer: Good. Verbose output stays isolated; only failures come back.

14. **Researching three independent modules: what pattern?**
    Answer: Spawn parallel subagents, one per module, then synthesize.

15. **How do you give a subagent team conventions from a skill at startup?**
    Answer: List the skill in its `skills` field so the full skill content is preloaded.
