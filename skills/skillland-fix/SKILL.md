---
name: skillland-fix
description: Fixes what the Skillland validator finds in an agent skill (a SKILL.md folder), editing the files and re-checking until the skill scores A. Use when the user asks to fix a skill's findings or raise its Skillland score.
license: MIT
disable-model-invocation: true
---

# Skillland fix

Edits a skill until the Skillland validator grades it A. This skill changes files, so it runs only when the user invokes it by name: `/skillland-fix` in Claude Code and Cursor, `$skillland-fix` in Codex.

## Steps

1. Run the check on the skill folder, the same command `skillland-validate` uses:

   ```bash
   npx -y -p @skillland/validator@0.6.0 skillland check <path> --json
   ```

   If a report for the same files is already in the conversation, reuse it. Note the starting score and grade.
2. Fix the findings in order: errors, then warnings, then suggestions. Each finding carries a `fix` line. When it isn't enough, read the rule page at `https://skillland.io/validate/rules/<ruleId>`, which has a failing and a passing example.
3. Change only what a finding names, and keep what the skill does. Some fixes need facts only the user has, such as when the skill should trigger or a real example of its output. Ask for those in one question. Don't invent them.
4. Run the check again. Repeat steps 2 and 3 until the grade is A with no errors, or after three rounds.
5. Report the score before and after, each change in one line, and any finding left with the reason it stays.

## Example

```text
pdf-forms: from 82 (B) to 100 (A)

- Description now says what it saves and ends with "Use when the user asks to fill in a PDF form." (description-no-trigger, description-too-short).
- Added an example: a one-field JSON file and the filled copy it gives (no-examples).

No findings left.
```

## Hold these lines

- Don't delete content to pass a rule. When the body is too long, move sections into `references/` and link them from `SKILL.md`.
- Don't remove a vendor field such as `disable-model-invocation` or `paths` only to clear the `non-spec-fields` suggestion. It costs 2 points and the field does real work.
- Don't touch files outside the skill folder.
- Show the user the diff of `SKILL.md` before the final report.
