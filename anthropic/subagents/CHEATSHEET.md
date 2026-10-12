# Introduction to Subagents: Cheat Sheet

Course: https://anthropic.skilljar.com/introduction-to-subagents (ORM-294)
Sources: [Claude Code subagents docs](https://code.claude.com/docs/en/sub-agents), [Skills docs](https://code.claude.com/docs/en/skills)

Written from public docs in our own words. Not a quiz key.

## Core concepts
- **Subagent**: a specialized assistant that runs in its own fresh context window with its own system prompt, tool access and permissions, then returns a summary to the main conversation.
- **Why**: keep noisy work (searches, logs, test output) out of the main context; enforce tool limits; reuse specialist setups.
- **Input**: the subagent does not see your chat history. It gets its system prompt plus the delegation message Claude writes (plus CLAUDE.md and git status for most types).
- **Output**: only the final report comes back. Intermediate tool calls stay in the subagent.
- **Create**: `/agents` command or a markdown file with YAML frontmatter in `.claude/agents/` (project) or `~/.claude/agents/` (user).

## File format
```markdown
---
name: code-reviewer          # required
description: Reviews diffs. Use proactively after code changes.   # required
tools: Read, Grep, Glob      # optional allowlist, omit to inherit all
model: sonnet                # optional: sonnet, opus, haiku, inherit
---
System prompt goes here.
```
Other fields: `disallowedTools`, `permissionMode`, `skills`, `mcpServers`, `hooks`, `maxTurns`, `memory`, `isolation: worktree`, `color`.

## Built-in subagents
- **Explore**: fast, read-only codebase search.
- **Plan**: read-only research used in plan mode.
- **general-purpose**: full tools for multi-step work.

## Designing effective subagents
- One focused job per subagent.
- Description says when to delegate; "use proactively" encourages automatic use.
- Limit tools to what the job needs (a reviewer gets no Edit or Write).
- Define a structured output format so the report is easy to use.
- Ask it to report obstacles and partial results instead of looping silently.
- Give it a clear stopping point.

## 10 facts most likely tested
1. Each subagent has its own isolated context window.
2. It returns a summary, which keeps the main context clean.
3. It does not inherit conversation history; the delegation prompt must carry the needed context.
4. `name` and `description` are the required frontmatter fields.
5. Claude chooses a subagent mainly from its description.
6. Omitting `tools` inherits all available tools; listing them restricts.
7. Project subagents live in `.claude/agents/`, user ones in `~/.claude/agents/`; project wins on a name clash.
8. `/agents` is the entry point for creating and managing them.
9. You can invoke one explicitly by naming it or with an @-mention.
10. Subagents cost their own tokens and add startup latency.

## When to use, when not to
Use: verbose output you will not reread, parallel independent research, strict tool limits, self-contained tasks.
Avoid: quick edits, tight back-and-forth, phases that share lots of context (plan, implement, test), latency-sensitive steps.

## Common gotchas
- Assuming the subagent knows what you discussed. It does not.
- Too many overlapping descriptions, so Claude routes to the wrong one.
- Giving a reviewer write tools.
- Spawning many subagents that each return long reports, which refills the main context anyway.
- Expecting a subagent to ask you questions mid-task; it should report back instead.
