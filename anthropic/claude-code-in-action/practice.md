# Claude Code in Action: Practice Questions

Self-made from the public docs. Not the course quiz.

1. **You are about to start a large refactor. What mode first, and why?**
   Answer: Plan mode. Claude explores read-only and proposes a plan you can correct before any file changes.

2. **How do you switch permission modes during a session?**
   Answer: Shift+Tab cycles them.

3. **Context is filling up mid-task. What do you run, and what makes it better?**
   Answer: `/compact`, with instructions on what to keep, such as decisions made, files touched and open problems.

4. **Claude went down a wrong path five turns ago. Fastest recovery?**
   Answer: `/rewind` (or Esc twice) to restore code and conversation to an earlier checkpoint.

5. **Can rewind undo a production deploy Claude ran?**
   Answer: No. Checkpoints track Claude's file edits, not external side effects.

6. **What belongs in CLAUDE.md, and what does not?**
   Answer: Short, always-relevant facts: build and test commands, conventions, gotchas. Long step-by-step procedures belong in skills.

7. **Rule: never run `rm -rf`. CLAUDE.md line or hook?**
   Answer: Hook. A `PreToolUse` hook on Bash that exits 2 blocks it every time.

8. **What does exit code 2 mean for a PreToolUse hook?**
   Answer: Block the tool call; stderr is shown to Claude as the reason.

9. **How do you make Claude keep working until tests pass?**
   Answer: A `Stop` hook that runs the tests and blocks stopping while they fail.

10. **Which mode auto-accepts file edits but still asks about other actions?**
    Answer: `acceptEdits`.

11. **How do you run Claude Code from a CI script?**
    Answer: Headless mode, `claude -p "..."`, with `--output-format json` for machine-readable output and pre-approved tools.

12. **How do teammates trigger Claude on a pull request?**
    Answer: Mention `@claude` in a PR or issue comment once the Claude GitHub app or `claude-code-action` is installed.

13. **A scheduled cloud routine says a skill is not found. Likely cause?**
    Answer: The skill exists only in your personal `~/.claude/skills`. Commit it to the repo's `.claude/skills/` or enable it on your claude.ai account.

14. **What can a plugin bundle?**
    Answer: Skills, subagents, hooks, MCP servers and commands, installed together with `/plugin`.

15. **Claude reports "all done, tests pass" after an unattended hour. What do you do?**
    Answer: Verify in proportion: read the diff, rerun the tests yourself or check the hook results, and spot-check the running app.
