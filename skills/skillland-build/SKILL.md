---
name: skillland-build
description: Builds a new agent skill for Claude Code, Codex or Cursor by asking the questions of the Skillland skill builder, writing the SKILL.md folder where the agent loads it and checking it with the Skillland validator. Use when the user asks to build a skill with Skillland or names skillland-build.
license: MIT
---

# Skillland build

Turns a few answers into a skill folder that scores A on the Skillland validator. It asks what the form at https://skillland.io/validate/builder asks and writes the same layout, then saves the folder where the user's agent looks for skills.

## Steps

1. Ask the questions in [references/questions.md](references/questions.md) in one message. Fill in what the conversation already answers and ask only for the rest. The name, what the skill does, when to use it and the steps are required.
2. Pick the folder from the agent and scope the user chose:

   | Agent | Project | Personal |
   | --- | --- | --- |
   | Claude Code | `.claude/skills/<name>/` | `~/.claude/skills/<name>/` |
   | Codex | `.agents/skills/<name>/` | `~/.agents/skills/<name>/` |
   | Cursor | `.cursor/skills/<name>/` | `~/.cursor/skills/<name>/` |

   For a project used with more than one agent, `.agents/skills/` is read by both Codex and Cursor, and Cursor also reads `.claude/skills/`. If the folder already exists, ask before writing over it.
3. Write `SKILL.md` in the layout of [references/skill-template.md](references/skill-template.md). Build the description as "<what it does>. Use when <when>." and leave out any optional field the user skipped.
4. When the skill runs only by name and Codex is one of its agents, also write `agents/openai.yaml` with `policy: allow_implicit_invocation: false`. Write any scripts to `scripts/` and reference files to `references/`, and list each one under `## Files`.
5. Check the folder:

   ```bash
   npx -y -p @skillland/validator@0.6.0 skillland check <folder> --json
   ```

   Fix each finding by its `fix` line and check again until the grade is A, at most three rounds. If a fix needs a fact the user hasn't given, ask for it instead of making it up.
6. Report the folder path, the score and grade, and how to call the skill: `/<name>` in Claude Code and Cursor, `$<name>` in Codex.

## Example

The user says: "build a skill that writes release notes from merged PRs, for Claude Code in this repo".

After the questions, the skill writes `.claude/skills/release-notes/SKILL.md`:

```markdown
---
name: release-notes
description: Writes release notes from the pull requests merged since the last tag. Use when the user asks for release notes or a changelog entry.
---

# Release notes

## Steps

1. Find the last tag with `git describe --tags --abbrev=0`.
2. List the pull requests merged since that tag.
3. Group them under Added, Changed and Fixed.

## Example

Input: three merged PRs since v1.2.0.
Output: a "## 1.3.0" section with one line per PR under its group.
```

Then it reports: `.claude/skills/release-notes/`, 100 (A), call it with `/release-notes`.

## Notes

- The questions follow the builder page's form. When the form changes, update `references/questions.md` too.
- An A means the structure follows the official guidance. It doesn't mean the skill does its job well, so suggest the user try it on a real task.
