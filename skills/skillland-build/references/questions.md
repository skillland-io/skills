# Questions for a new skill

The same fields as the form at https://skillland.io/validate/builder, in its order. Ask them in one message and skip any the conversation already answers.

## Required

1. **Name.** Lowercase letters, digits and hyphens, at most 64 characters, for example `pdf-forms`. It is also the folder name.
2. **What it does.** One sentence in the third person: "Fills PDF forms from a JSON file."
3. **When to use it.** The requests or situations that should load it: "the user asks to fill or read a PDF form."
4. **Steps.** What the agent does, in order, one step per line.

## Optional

5. **Example.** One real input and the output it should give.
6. **Allowed tools.** Tools the skill may use without asking, such as `Bash(python scripts/fill.py *)`. Leave empty unless the user needs it, and never pre-approve plain `Bash` or `*`.
7. **Run only by name.** Yes when the skill deploys, publishes, sends, deletes or pushes, so the agent never starts it alone. This sets `disable-model-invocation: true`.
8. **License.** For example `MIT`, when the skill will be shared.
9. **Title.** The heading of `SKILL.md`. Defaults to the name.
10. **Scripts.** File names and contents for `scripts/`. Scripts take flags and never wait for typed input.
11. **References.** File names and contents for `references/`, for detail the agent reads only when needed.
12. **Notes.** Anything else the agent must know, such as limits or edge cases.

## Where it goes

13. **Agent.** Claude Code, Codex, Cursor, or more than one.
14. **Scope.** This project only, or every project on this machine.
