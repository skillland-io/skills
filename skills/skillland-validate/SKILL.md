---
name: skillland-validate
description: Checks an agent skill (a SKILL.md folder) against the Skillland validator rules and reports its score, grade and each finding with a link to the rule. Use when the user asks to check, lint, review or score a skill, or right after a SKILL.md was written or edited.
license: MIT
---

# Skillland validate

Runs the open Skillland validator on one or more skills and reports what it finds. This skill only reads files. To change the skill, the user runs `skillland-fix`.

## Steps

1. Find the skill folder, the folder that holds `SKILL.md`. If the user gave no path, look in `.claude/skills/`, `.agents/skills/` and `.cursor/skills/`. If more than one skill is there and the user meant one, ask which. A parent folder checks every skill below it.
2. Run the validator. It needs Node 20 or later:

   ```bash
   npx -y -p @skillland/validator@0.7.0 skillland check <path> --json
   ```

   Exit code 1 only means a skill has an error; the JSON is still printed. Exit code 2 means a bad path or option. If `npx` itself fails, show its error and stop. Never estimate a score by reading the file yourself.
3. Read the JSON. It is a list with one entry per skill, each with `path` and `report`. In `report`, use `score` (0 to 100), `grade` (A to F), `areas` (one grade per area: Specification, Description, Structure and content, Security) and `findings` (each with `ruleId`, `severity`, `message`, `fix` and `line`).
4. Report each skill in this order: the score and grade, the four area grades, then the findings with errors first, warnings next and suggestions last. For each finding give the line, the message, the fix and the rule page at `https://skillland.io/validate/rules/<ruleId>`.
5. When a skill has no findings, say so in one line. When it has findings, end by naming `skillland-fix` as the way to fix them.

## Example

The user asks: "check my pdf skill".

```text
pdf-forms (.claude/skills/pdf-forms): 82, grade B
Specification A, Description B, Structure and content A, Security A

Warning, line 3: No "Use when" in the description.
Fix: Name the requests or situations that should load the skill.
Rule: https://skillland.io/validate/rules/description-no-trigger

Warning, line 3: Description is too short (49 characters).
Fix: Write what the agent produces and the situation that calls for it.
Rule: https://skillland.io/validate/rules/description-too-short

Suggestion: No example or code block in the body.
Fix: Show a real input and the output the skill should produce.
Rule: https://skillland.io/validate/rules/no-examples

Run skillland-fix to fix these.
```

## Notes

- The score measures structure against official guidance: the Agent Skills specification and the skills docs from Anthropic, OpenAI and Cursor. It does not show whether the skill does its job well, so don't say it does.
- The validator version is pinned, so results stay the same until the skill is updated. Every rule, its source quote and the scoring formula are at https://skillland.io/validate/rules.
