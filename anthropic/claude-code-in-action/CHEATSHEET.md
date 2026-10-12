# Claude Code in Action: Cheat Sheet

Course: https://anthropic.skilljar.com/claude-code-in-action (ORM-297)
Sources: [Claude Code docs](https://code.claude.com/docs/en/overview), [Common workflows](https://code.claude.com/docs/en/common-workflows), [Memory / CLAUDE.md](https://code.claude.com/docs/en/memory), [Permission modes](https://code.claude.com/docs/en/permission-modes), [Hooks](https://code.claude.com/docs/en/hooks), [Headless](https://code.claude.com/docs/en/headless), [GitHub Actions](https://code.claude.com/docs/en/github-actions), [Plugins](https://code.claude.com/docs/en/plugins), [Checkpointing](https://code.claude.com/docs/en/checkpointing), [Skills](https://code.claude.com/docs/en/skills)

Written from public docs in our own words. Not a quiz key. The course was rebuilt around long, unsupervised runs; its four units are below.

## 1. Steer the work
- **Plan mode**: read-only research and a proposed plan before any edits. Cycle modes with Shift+Tab. Use it to scope big tasks.
- **Compaction**: `/compact <what to keep>` summarizes history to free context. Give it instructions so the summary keeps decisions, file paths and open issues.
- **Rewind**: `/rewind` (or Esc twice) restores code and or conversation to a checkpoint. Checkpoints track Claude's file edits, not external side effects like deploys or DB writes.
- **Hands-on vs autonomous**: steer turn by turn for ambiguous work; hand off goal or loop style runs (for example the `/loop` skill) when the finish line is clear and checkable.

## 2. Configure Claude
- **CLAUDE.md**: always-loaded project memory. Keep it short and factual (commands, conventions, gotchas). `/init` drafts one. Locations: project root, `~/.claude/CLAUDE.md` for personal, nested ones load for subfolders. Long procedures belong in skills.
- **Skills**: repeatable procedures loaded on demand, such as a verification skill that builds, runs and checks the change.
- **Permission modes**: `default` (ask), `acceptEdits` (auto-accept edits), `plan` (read-only), `auto` (classifier reviews actions), `dontAsk` (deny anything not pre-allowed), `bypassPermissions` (no prompts, sandboxes only).
- **Hooks**: shell commands on lifecycle events (`PreToolUse`, `PostToolUse`, `Stop`, `UserPromptSubmit`, `SessionStart` and more). Deterministic, so they enforce rules Claude cannot skip. A `PreToolUse` hook that exits with code 2 blocks the tool call and feeds stderr back to Claude.

## 3. Automate repeat work
- **Routines / scheduled tasks**: prompts that run on a schedule, in the cloud or locally. Cloud runs only see skills committed to the repo or synced to the account.
- **Headless mode**: `claude -p "prompt"` runs non-interactively; `--output-format json` or `stream-json` for pipelines; pair with `--allowedTools` to pre-approve tools.
- **GitHub**: the Claude GitHub app and `anthropics/claude-code-action` let `@claude` in issues and PRs implement or review; managed code review posts review comments on PRs. `/install-github-app` sets it up.

## 4. Verify and share
- **Verify in proportion**: the less you watched, the more you check. Read the diff, run tests, run the app, check the claims in the summary.
- **Gate on real results**: a `Stop` hook can run the tests and block the turn from ending until they pass.
- **Plugins**: bundle skills, subagents, hooks, MCP servers and commands into one installable unit. Distribute via a marketplace; install with `/plugin`.

## 10 facts most likely tested
1. Plan mode researches and proposes without editing files.
2. Shift+Tab cycles permission modes.
3. `/compact` accepts instructions about what to keep.
4. `/rewind` restores to checkpoints; it cannot undo external side effects.
5. CLAUDE.md is always loaded, so keep it lean; procedures go to skills.
6. Hooks are deterministic; instructions are suggestions. Use hooks for non-negotiables.
7. Exit code 2 from a `PreToolUse` hook blocks the action.
8. `claude -p` is headless mode for scripts and CI.
9. The GitHub action responds to `@claude` mentions in PRs and issues.
10. Plugins package skills, agents, hooks and MCP servers for team install.

## Common gotchas
- A huge CLAUDE.md gets ignored in practice and costs tokens every turn.
- Trusting the end-of-run summary without checking the diff or tests.
- `bypassPermissions` outside a sandbox.
- Relying on rewind to undo a deploy or a database change.
- Personal `~/.claude/skills` are not available to cloud routines.
