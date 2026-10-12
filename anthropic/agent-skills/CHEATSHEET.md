# Introduction to Agent Skills: Cheat Sheet

Course: https://anthropic.skilljar.com/introduction-to-agent-skills (ORM-293)
Sources: [Claude Code skills docs](https://code.claude.com/docs/en/skills), [Agent Skills overview](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview), [Skill authoring best practices](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/best-practices)

Written from public docs in our own words. Not a quiz key.

## Core concepts
- **Skill**: a folder with a `SKILL.md` file (YAML frontmatter plus markdown instructions) and optional supporting files. Teach Claude once instead of re-pasting the same procedure.
- **Frontmatter**: metadata between `---` markers on line 1. `description` is the field that matters most; `name` defaults to the folder name.
- **Progressive disclosure**: only names and descriptions sit in context all the time. The full SKILL.md loads when the skill is used, and extra files (reference.md, scripts) load or run only when needed.
- **Scripts**: bundled scripts are executed, not read into context, so their code costs no tokens; only output comes back.
- **Invocation**: Claude picks a skill automatically by matching the request to the description, or you type `/skill-name`.

## Where skills live (scope)
| Location | Path | Reach |
|---|---|---|
| Enterprise | managed settings dir | whole org |
| Personal | `~/.claude/skills/<name>/SKILL.md` | all your projects |
| Project | `.claude/skills/<name>/SKILL.md` | the repo, shared by committing |
| Plugin | `<plugin>/skills/<name>/SKILL.md` | wherever the plugin is enabled, as `/plugin:skill` |

Same name in several places: enterprise beats personal, personal beats project. Plugin skills are namespaced so they do not clash.

## Key frontmatter fields
- `description`: what it does plus when to use it. Front-load the use case and trigger words.
- `allowed-tools`: tools Claude may use without asking while the skill runs. It pre-approves; it does not restrict the rest.
- `disable-model-invocation: true`: only you can trigger it (good for deploy, send, commit).
- `user-invocable: false`: only Claude can trigger it (background knowledge).
- `context: fork` plus `agent`: run the skill inside a subagent with its own context.
- `model`, `paths`, `argument-hint`, `hooks` are Claude Code extras.

## Skills vs other features
- **CLAUDE.md**: always loaded facts and conventions. Skills: procedures loaded on demand.
- **Hooks**: deterministic, run on every event. Use for rules that must never be skipped.
- **Subagents**: separate context window for isolated work. Skills run in the main context unless forked.
- **MCP**: connects external tools and data. Skills teach how to use them.

## 10 facts most likely tested
1. A skill is a directory whose required file is `SKILL.md`.
2. The description drives automatic triggering, so it must say what and when.
3. Progressive disclosure: metadata always, body on use, extra files on demand.
4. Keep SKILL.md lean (docs suggest under about 500 lines); push detail into linked files.
5. Scripts run without their source entering the context window.
6. `allowed-tools` grants permission for listed tools; it is not a sandbox.
7. Share with a team by committing `.claude/skills/` to the repo; wider via plugins; org-wide via managed settings.
8. Personal skills live in `~/.claude/skills/`.
9. Subagents can preload skills with the `skills` field; skills can run in a subagent with `context: fork`.
10. Custom slash commands (`.claude/commands/`) were merged into skills and still work.

## Common gotchas
- Vague description ("helps with files") means the skill never triggers.
- Frontmatter not on line 1, or broken YAML, means metadata is ignored.
- Name collision: a personal skill silently wins over the project one.
- Too many skills: descriptions get trimmed from the listing to fit budget.
- Must-always rules belong in hooks, not in a skill Claude may forget after compaction.

## Troubleshooting order
Not triggering: sharpen description keywords, check it is listed, invoke directly, run with `--debug` for YAML errors. Triggering too often: narrow description or set `disable-model-invocation`.
