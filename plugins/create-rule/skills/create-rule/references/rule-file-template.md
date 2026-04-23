# Rule File Template

```markdown
---
paths:
  - "src/path/to/scope/**/*.ts"
---

# [Convention Name]

One imperative sentence: what this rule enforces and why it exists.

## [Concern A]

Direct commands. ✅/❌ contrast only when the distinction is non-obvious.

## [Concern B]

…

## Anti-patterns

- NEVER … (things Agent B found that aren't already forbidden by the positive rules above)

## Full Example

@.claude/rules/<feature>/examples/<name>.ts
```

**Frontmatter `paths:`** — must be a glob that matches exactly the files this rule governs. Too broad = noise on unrelated tasks.
