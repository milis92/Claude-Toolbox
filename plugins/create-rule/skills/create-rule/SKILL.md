---
name: create-rule
description: >
  Use when the user wants to create, update, or generate a .claude/rules/ file
  for any coding convention topic (testing, error handling, architecture, git
  workflow, etc.). Also use when the user wants to codify a pattern Claude keeps
  getting wrong. Do NOT trigger for writing CLAUDE.md files, reviewing existing
  rules, linter/prettier configs, CONTRIBUTING.md, writing tests, fixing bugs,
  or refactoring.
argument-hint: [ topic ]
allowed-tools: Read, Write, Edit, Glob, Grep, Agent
---

# Writing Rule Files

Generate focused `.claude/rules/<topic>.md` files by analyzing existing code patterns, confirming findings with the user, and self-validating via an eval loop.

## Checklist

Create a task for each of these items and complete them in order:

1. **Clarify topic if vague** — if the user says "write a rule file" without a specific topic, ask what area they want to document. Don't try to document an entire project in one file.
2. **Infer globs from topic** — map the user's topic to file patterns
3. **Glob and sample files** — find matches, sample 5-10 from different modules
4. **Read and identify patterns** — read files in parallel, find non-obvious project-specific conventions only
5. **Present findings to user** — show candidate patterns + exemplar files; confirm both the files and which patterns are worth turning into rules
6. **Determine target filename** — apply kebab-case convention; check if the file already exists
7. **Scan ALL existing rules for overlap AND contradiction** — read every `.claude/rules/*.md` and `CLAUDE.md`; deduplicate overlaps, surface contradictions to user before proceeding
8. **Write rule file** — write to `.claude/rules/<topic>.md` only after any contradictions are resolved
9. **Eval loop** — launch subagent in worktree to validate the rule
10. **Completion** — confirm file path, remind user to commit, check line count, suggest `/claude-md-improver`

## Phase 1: Discovery

### Handle User-Provided Conventions

Sometimes the user already knows the conventions and states them directly (e.g., "We use thiserror for library errors and anyhow for app errors. Never use unwrap() in production."). When this happens:

- Accept the user's stated conventions as the primary source of truth
- Still run discovery to find exemplar files you can reference with `@file/path`
- Only surface additional patterns Claude has already gotten wrong or that the user has had to correct — do not speculatively hunt for more conventions to document
- Verify the codebase actually follows the stated conventions (if it doesn't, flag the discrepancy)

### Infer Glob Patterns

Map the user's topic to likely file patterns. Adapt to whatever language/framework the project uses:

- "unit testing" in TS -> `**/__tests__/**/*.test.{ts,tsx}`, `**/*.spec.ts`
- "unit testing" in Kotlin -> `**/src/test/**/*.kt`, `**/*Test.kt`
- "unit testing" in Go -> `**/*_test.go`
- "API endpoints" -> `**/handler/**/*.go`, `**/controller/**/*.java`, `**/routes/**/*.ts`
- "error handling" -> `**/error*/**`, `**/exception/**`, then broaden with grep for error/Result/throw patterns

If the topic doesn't map to obvious patterns, ask the user.

### Sample Files

- Sort matches by modification time (recent = more representative of current conventions)
- Pick 5-10 files from different modules for cross-cutting coverage
- If fewer than 3 files match, widen the search or ask the user
- If more than 50 match, narrow by selecting from distinct directories

### Identify Patterns

Read sampled files in parallel. Focus on patterns that are **non-obvious and project-specific** — things Claude has already gotten wrong, or that the user has had to correct. Ask yourself: "Has Claude violated this? Or has the user corrected this in chat before?" That is the reactive bar. A secondary check: "Would Claude write this differently from scratch?" — but prefer patterns with actual evidence of past mistakes over predictions.

Good patterns to document:
- Project-specific choices that override language/framework defaults (e.g., stubs instead of mocks, a custom error type instead of standard errors)
- Constraints that are easy to violate (e.g., "never use unwrap()", "always use the response envelope")
- Architectural boundaries that deviate from the framework's expected layout — not generic "keep controllers thin" advice
- Non-obvious framework/library usage (custom wrappers, specific annotation patterns)

Skip patterns that are standard for the language/framework — Claude already knows those. When in doubt: has Claude already gotten this wrong, or has the user corrected it? If yes, document it. If not, skip it — pre-empting mistakes is the primary source of rule bloat.

### Present to User

Show a summary: "I found these patterns across these files: [list]. Do these files look representative? And which of these patterns are worth turning into a rule — i.e., things Claude has actually gotten wrong, or that you've had to correct more than once?"

Wait for confirmation on both the files and the candidate patterns. If the user redirects, adjust and re-sample. Don't proceed to writing until the user has validated that the patterns meet the bar — pre-emptive documentation of things Claude hasn't violated yet is the primary source of rule bloat.

## Phase 2: Write Rule File

### Infer `paths:` Frontmatter

Decide whether the rule should be contextually loaded or always loaded:

- Topic maps to specific file locations -> generate `paths:` globs
- Topic is cross-cutting or architectural -> omit `paths:` (always loaded)

!IMPORTANT: Always include `paths:` frontmatter when the topic applies to specific file types. Without it, the rule loads every session and wastes context tokens.

**Glob precision matters.** Too broad = rule fires on unrelated files and wastes context. Match exactly the files this rule governs:

| Pattern                | Matches                                        |
|------------------------|------------------------------------------------|
| `**/*.ts`              | All TypeScript files anywhere                  |
| `src/api/**/*.ts`      | TypeScript under `src/api/` only               |
| `src/**/*.{ts,tsx}`    | TypeScript and TSX under `src/`                |
| `*.md`                 | Markdown in project root only                  |
| `src/components/*.tsx` | TSX directly in `src/components/` (no subdirs) |

Prefer the narrowest glob that captures all relevant files.

### Scan for Overlap with Existing Rules

Before writing, read ALL files in `.claude/rules/` and `CLAUDE.md` — skipping this leads to contradictory or duplicated guidance.

For each existing rule file, check for two failure modes:

1. **Overlap** — another file already covers the same pattern. If the exact topic file exists (`.claude/rules/<topic>.md`): ask the user — overwrite, merge, or rename? If a *different* rule file overlaps: actively remove the duplicate content from your new file, then add a `<!-- -->` comment (zero token cost) noting the boundary, e.g.:
   ```
   <!-- Complements api.md (HTTP-side error mapping). This file covers domain error types only. -->
   ```
2. **Contradiction** — another file says something that conflicts with what you plan to write (e.g., one rule says "never use X", another says "prefer X here"). Surface the conflict to the user before writing; do not silently pick one side.

If merge: match sections by heading, replace matched sections, append new ones.

### Filename Convention

Use lowercase-kebab-case with broadest category first:

- "unit testing" -> `testing-unit.md`
- "instrumented testing" -> `testing-instrumented.md`
- "dependency injection" -> `dependency-injection.md`

Check existing rule filenames to match the project's naming convention.

### Rule File Template

@plugins/create-rule/skills/create-rule/references/rule-file-template.md

When the rule includes a non-trivial example, create it as a real file at the `@` path shown in the template — the `@` syntax auto-loads it into context on every run. Place example files in `.claude/rules/<feature>/examples/`.

### Writing Principles

**Imperative, not descriptive.**
- ✅ `Always export the ID type as a plain string alias.`
- ❌ `ID types are exported as string aliases.`

**Specific, not vague.** Imperative + vague = useless. The rule must be concrete and verifiable:
- ✅ `Always return Result<T, AppError> from service-layer functions.`
- ✅ `Run npm test before committing.`
- ✅ `API handlers live in src/api/handlers/.`
- ❌ `Always handle errors properly.`
- ❌ `Test your changes.`

**Write rules reactively.** Add a rule when Claude makes the same mistake twice, or when you find yourself typing the same correction in chat for the second time. Don't pre-document conventions Claude hasn't violated yet.

**Rules are context, not enforcement.** Claude.md content is loaded into the context window — it influences Claude's behavior but is not machine-enforced. This is why specificity and imperative phrasing matter: vague instructions have lower adherence than concrete ones.

**Rules before examples.** State the constraint first so the model interprets the example through the rule.

**Anti-patterns enforce, don't restate.** If the positive rule already says "do X", don't add "NEVER do not-X". Anti-patterns are for temptations the positive rules don't cover.

**Single-item sections are noise.** Fold into an adjacent section or the anti-patterns list.

**`@filename` makes examples live.** Prose can be terse because the example is always present. Remove any inline snippet that just restates what the example shows; keep snippets only for ✅/❌ contrasts.

**Use `<!-- -->` for maintainer notes.** Context that helps humans understand *why* a rule exists but isn't instructions for Claude belongs in an HTML comment — it consumes zero tokens:
```
<!-- This rule exists because the legacy auth middleware stored session tokens in plaintext. -->
```

### What to Cut

| If you have…                                  | Cut it because…            |
|-----------------------------------------------|----------------------------|
| A section with one bullet                     | Fold into adjacent section |
| An anti-pattern restating a positive rule     | Redundant — remove         |
| An inline snippet covered by the example file | Remove — example is live   |
| Prose explaining what a code example shows    | The example shows it       |
| `!IMPORTANT` on more than one thing           | Dilutes signal — pick one  |

### Content Rules

**Include:** build/test commands Claude can't guess, naming conventions, project layout, style rules that differ from language defaults, architectural boundaries, API conventions (response envelopes, auth patterns, custom wrappers), common workflows, "always do X" rules — plus, for rule files specifically: environment quirks, `!IMPORTANT` for constraints Claude might skip, and `@file/path` references to exemplar code.

**Exclude:**
- Anything Claude can infer from reading code
- Standard language/framework conventions
- Multi-step procedures → put in a skill instead
- Instructions that only apply to one part of the codebase → use path-scoped rules instead
- Frequently changing information (goes stale and erodes trust in the file)
- Detailed API docs or tutorials

The litmus test: *would Claude do this differently without the rule?* If no, cut it.

**Line count:** The root `CLAUDE.md` should stay under 200 lines — beyond that, adherence degrades. Scoped rule files in `.claude/rules/` should be shorter still. Every token loaded into context costs adherence, so shorter is always better.

## Phase 3: Eval Loop

After writing the rule file, validate it by testing whether a fresh agent follows the conventions.

### Task Selection

- **Path-scoped rules (`paths:` present):** pick a file matching the glob; choose a small task (write a test, add an endpoint, scaffold a screen). Also pick one file that should NOT match the glob and verify the rule doesn't fire there.
- **Cross-cutting rules (no `paths:`):** any file qualifies — pick one where the conventions are easy to violate. Verify the rule fires on multiple file types, not just one.
- Prefer files NOT used during discovery (avoids copying from exemplars)
- If the topic is too abstract for a concrete task, ask the user

### Run Eval

1. Launch a subagent with `isolation: "worktree"` and a concrete task. The subagent runs with full project context (CLAUDE.md + all rules). Do NOT inline the rule content — the point is to test whether the rule file works in-situ.

2. Review the subagent's output against each convention in the rule:
    - Does the code use the specified frameworks/libraries/annotations?
    - Does naming follow the documented conventions?
    - Are `!IMPORTANT` constraints respected?
    - Are prohibited patterns absent (not just correct patterns present)?
    - Does file organization / style match documented conventions?
    - Would the code work with the project's build system?

3. If all conventions followed: done.

4. If gaps found: determine the cause before updating:
    - *Rule was too vague* → make the instruction more concrete and specific
    - *Convention was missing* → add it to the rule
    - *Rule was never loaded* → check that the `paths:` glob matches the eval file; fix if misconfigured
    Re-launch a fresh eval (new worktree) after updating.

5. Max 3 iterations. After 3 failures, report to the user: which specific conventions the subagent violated, whether the rule was vague or missing coverage, and whether breaking the rule into smaller scoped files might improve adherence.

### Error Handling

- Worktree creation fails: skip eval, warn user ("eval skipped — recommend manually testing the rule by asking Claude to write [X] and checking it follows the conventions"), recommend manual review
- Subagent produces output clearly unrelated to the task: retry with a more constrained task description
- Subagent produces no output: count as failed iteration, retry with simpler task
- No suitable eval task: skip eval, inform user

## Phase 4: Completion

Tell the user:

1. **File written:** "Rule file written to `.claude/rules/<topic>.md`."
2. **Commit it:** "If this is a project-level rule, commit the file so the team shares it: `git add .claude/rules/<topic>.md && git commit -m 'add <topic> rule'`."
3. **Check length:** "The rule file is N lines. Keep scoped rule files shorter than root `CLAUDE.md` (which Anthropic recommends under 200 lines) — trim anything that doesn't pass the litmus test."
5. **Optional:** "Consider running `/claude-md-improver` (if available) after adding rules to review the full setup."
