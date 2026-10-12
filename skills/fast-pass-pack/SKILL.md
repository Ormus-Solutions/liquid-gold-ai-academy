---
name: fast-pass-pack
description: Build a fast study pack (cheat sheet, key ideas, practice questions) for an online course from its public outline and official docs. Use when the user wants to learn or pass a course quickly, or asks for a cheat sheet or study guide for a course.
---

# Fast-pass study pack

Input: course URL and its topic.

1. Read the public course page. Record the curriculum headings and learning goals only.
2. Map each heading to official public docs on the same topic. Prefer the vendor's own docs and specs.
3. Write `CHEATSHEET.md` (one page): core concepts, a definitions table, the 10 facts most likely tested, common gotchas, source links.
4. Write `key-ideas.md`: 5 to 10 principles in your own words.
5. Write `practice.md`: 12 to 15 original questions with short explained answers, each grounded in the docs.
6. Update the progress tracker to `pack ready`.

Rules:
- Never obtain, guess at, or publish the real quiz questions or answer keys.
- Never copy course text word for word. Distill and link.
- Flag anything you could not verify against an official source.
- Plain style: no em dashes, no exclamation marks.
