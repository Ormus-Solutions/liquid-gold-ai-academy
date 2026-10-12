# Agent Skills: Practice Questions

Self-made from the public docs. Not the course quiz. Cover the answer, then check.

1. **What single file must every skill folder contain?**
   Answer: `SKILL.md`. Supporting files are optional.

2. **Which frontmatter field most affects whether Claude auto-uses a skill?**
   Answer: `description`. Claude matches requests against it, so it should state both the capability and when to use it.

3. **Explain progressive disclosure in one line.**
   Answer: Only skill names and descriptions sit in context; the body loads when invoked; linked files and scripts load or run only if needed. This keeps the context window small.

4. **A skill bundles a 2,000 line Python script. How much of it enters context when Claude runs it?**
   Answer: None of the source. Executed scripts return output only.

5. **You want a `/deploy` skill Claude must never trigger on its own. What do you set?**
   Answer: `disable-model-invocation: true`.

6. **Does `allowed-tools: Read Grep` stop Claude from using Bash?**
   Answer: No. It pre-approves the listed tools during the skill turn. Other tools still follow normal permissions. To remove tools use `disallowed-tools` or deny rules.

7. **Same skill name exists in `~/.claude/skills/` and the project `.claude/skills/`. Which runs?**
   Answer: The personal one. Order is enterprise, then personal, then project.

8. **Easiest way to give the whole team a skill for one repo?**
   Answer: Commit it under `.claude/skills/` in that repo.

9. **How do you distribute skills across many repos or teams?**
   Answer: Package them in a plugin, or deploy through enterprise managed settings for the whole org.

10. **Rule: "never edit files in /migrations". Skill or hook?**
    Answer: Hook. Hooks run deterministically on every matching event; a skill is guidance Claude can drift from.

11. **CLAUDE.md vs skill: where does a 10 step release checklist go?**
    Answer: A skill. CLAUDE.md is always loaded and should hold short facts; long procedures cost tokens every turn there.

12. **Skill does not trigger. First three checks?**
    Answer: Description includes the words users actually say; the skill shows in the available list; YAML frontmatter parses and starts on line 1 (use `--debug`).

13. **How can a subagent get a skill's full content at startup?**
    Answer: List it in the subagent's `skills` frontmatter field. The full skill body is injected, not just the description.

14. **What does `context: fork` do in a skill?**
    Answer: Runs the skill as a task inside a separate subagent context, which does not see the conversation history.

15. **Where should detailed API reference for a skill go?**
    Answer: A separate file such as `reference.md`, linked from SKILL.md, so it loads only when needed.
