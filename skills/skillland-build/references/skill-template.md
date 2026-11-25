# SKILL.md layout

The layout the Skillland builder writes. Replace each `<...>` and drop every line or section whose answer is empty.

```markdown
---
name: <name>
description: <what it does>. Use when <when to use it>.
license: <license>
allowed-tools: <allowed tools>
disable-model-invocation: true
---

# <title, or the name>

## Steps

1. <first step>
2. <second step>

## Example

<example>

## Files

- [scripts/<file>](scripts/<file>)
- [references/<file>](references/<file>)

## Notes

<notes>
```

- `disable-model-invocation: true` goes in only when the skill runs by name only.
- Links under `## Files` point one level down from `SKILL.md`, never deeper.
