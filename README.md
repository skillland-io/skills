# Skillland skills

Three agent skills for Claude Code, Codex and Cursor that build, check and fix other skills with the [Skillland validator](https://github.com/skillland-io/skill-validator). The validator has 40 rules, and each one quotes the official guidance it relies on: the [Agent Skills specification](https://agentskills.io/specification) and the skills docs from Anthropic, OpenAI and Cursor.

| Skill | What it does | Starts on its own |
| --- | --- | --- |
| [`skillland-validate`](skills/skillland-validate/SKILL.md) | Runs the validator on a skill and reports the score, the grade and each finding with its rule page. Changes no files. | Yes, when you ask to check or review a skill |
| [`skillland-fix`](skills/skillland-fix/SKILL.md) | Edits a skill and re-checks it until it scores A. | No, call it by name |
| [`skillland-build`](skills/skillland-build/SKILL.md) | Asks the questions of the [skill builder](https://skillland.io/validate/builder), writes the folder where your agent loads skills, then checks it. | When you ask to build a skill with Skillland |

The skills hold no copy of the rules. Each one runs `npx -y -p @skillland/validator@latest skillland check`, so a new rule reaches them with the next validator release. They need Node 20 or later.

## Install

With the [skills CLI](https://github.com/vercel-labs/skills):

```bash
npx skills add skillland-io/skills
```

Or copy a folder from `skills/` to where your agent looks for skills:

| Agent | Project | Personal |
| --- | --- | --- |
| Claude Code | `.claude/skills/` | `~/.claude/skills/` |
| Codex | `.agents/skills/` | `~/.agents/skills/` |
| Cursor | `.cursor/skills/` or `.agents/skills/` | `~/.cursor/skills/` |

## Use

- "Check my pdf-forms skill" loads `skillland-validate`.
- `/skillland-fix` in Claude Code and Cursor, or `$skillland-fix` in Codex, fixes what it found.
- "Build a skill with Skillland that writes release notes" loads `skillland-build`.

A grade measures structure against the official guidance. It doesn't tell you whether a skill does its job well, so try it on a real task too.

## Checks on this repo

CI runs the validator on every skill here and fails below 90, so each one stays at A:

```bash
npx -y -p @skillland/validator@latest skillland check skills --min-score 90
```

## License

MIT
