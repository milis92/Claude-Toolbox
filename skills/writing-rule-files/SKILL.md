---
name: writing-rule-files
description: >
  Use when the user wants to create, write, update, or generate a .claude/rules/
  file for any topic — testing, networking, error handling, dependency injection,
  UI patterns, architecture, git workflow, or any other coding convention. Trigger
  whenever the user mentions "rule file", "write a rule", "rules markdown",
  "document conventions", "create coding guidelines", ".claude/rules/", or wants
  Claude to analyze existing code patterns and capture them as persistent rules.
  Also use when the user says Claude keeps doing something wrong and wants to
  codify the correct approach, or when they want to update an existing rule file.
  Do NOT trigger for writing CLAUDE.md files, reviewing/auditing existing rules,
  setting up linters/eslint/prettier configs, creating CONTRIBUTING.md, writing
  tests, fixing bugs, or refactoring code — even if those tasks mention conventions.
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
4. **Read and identify patterns** — read files in parallel, find recurring conventions
5. **Present findings to user** — show patterns + exemplar files, ask for confirmation
6. **Scan ALL existing rules for overlap** — read every `.claude/rules/*.md` and `CLAUDE.md`, flag content overlap
7. **Write rule file** — write to `.claude/rules/<topic>.md`
8. **Eval loop** — launch subagent in worktree to validate the rule
9. **Recommend `/claude-md-improver`** — tell user to run it after all issues are resolved

## Phase 1: Discovery

### Handle User-Provided Conventions

Sometimes the user already knows the conventions and states them directly (e.g., "We use thiserror for library errors and anyhow for app errors. Never use unwrap() in production."). When this happens:

- Accept the user's stated conventions as the primary source of truth
- Still run discovery to find exemplar files you can reference with `@file/path`
- Look for additional patterns the user didn't mention but that complement their stated conventions
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

Read sampled files in parallel. Focus on patterns that are **non-obvious and project-specific** — things Claude would get wrong without the rule file. Ask yourself: "If I were writing new code in this project without this rule, what would I do differently from what the codebase actually does?"

Good patterns to document:
- Project-specific choices that override language/framework defaults (e.g., stubs instead of mocks, a custom error type instead of standard errors)
- Constraints that are easy to violate (e.g., "never use unwrap()", "always use the response envelope")
- Architectural boundaries and dependency rules between modules
- Non-obvious framework/library usage (custom wrappers, specific annotation patterns)

Skip patterns that are standard for the language/framework — Claude already knows those.

### Present to User

Show a summary: "I found these patterns across these files: [list]. Are these good examples, or should I look elsewhere?"

Wait for confirmation before proceeding. If the user redirects, adjust and re-sample.

## Phase 2: Write Rule File

### Infer `paths:` Frontmatter

Decide whether the rule should be contextually loaded or always loaded:

- Topic maps to specific file locations -> generate `paths:` globs
- Topic is cross-cutting or architectural -> omit `paths:` (always loaded)

!IMPORTANT: Always include `paths:` frontmatter when the topic applies to specific file types. Without it, the rule loads every session and wastes context tokens.

### Scan for Overlap with Existing Rules

!IMPORTANT: Before writing, read ALL files in `.claude/rules/` and `CLAUDE.md`. This is not optional — skipping it leads to contradictory or duplicated guidance.

For each existing rule file, check whether it already covers any of the patterns you plan to document. If overlap exists:

1. If the exact topic file exists (`.claude/rules/<topic>.md`): ask the user — overwrite, merge, or rename?
2. If a *different* rule file covers related content: **add a complementary reference** at the top of your rule file and scope your content to avoid duplication.

!IMPORTANT: The complementary reference is the single most important thing when existing rules overlap. Add it as the first line after the frontmatter, e.g.:
> This complements `api.md` (which covers HTTP-side error mapping in handlers) and `logging.md` (which covers structured error logging). This file focuses on domain error types and service-layer error conventions.

Then actively exclude any content the other rules already cover — don't just note the overlap, actually remove the duplicated sections.

If merge: match sections by heading, replace matched sections, append new ones.

### Filename Convention

Use lowercase-kebab-case with broadest category first:

- "unit testing" -> `testing-unit.md`
- "instrumented testing" -> `testing-instrumented.md`
- "dependency injection" -> `dependency-injection.md`

Check existing rule filenames to match the project's naming convention.

### Content Rules

**Include:**

- Build/test commands Claude can't guess
- Code style rules that differ from language defaults
- Architectural decisions and module dependency rules
- Repo conventions (branch naming, commit style, PR process)
- Environment quirks, common gotchas not obvious from code
- `!IMPORTANT` for critical constraints Claude might skip
- `@file/path` references to exemplar code instead of inlining large blocks

**Exclude:**

- Anything Claude can infer by reading code (standard conventions, obvious patterns)
- Detailed API docs (link to exemplar files instead)
- Frequently changing information
- Long tutorials or file-by-file descriptions

### Anti-Patterns to Avoid

These are common mistakes when writing rule files. Do NOT:

- **Inline large code examples.** Use `@file/path` references to real files in the codebase instead (e.g., `@src/domain/error.rs` or `@internal/handler/user_handler.go`). This keeps the rule file short and always points to current code. Inline only short snippets (< 10 lines) that illustrate a non-obvious pattern.
- **State the obvious.** "Tests live in `src/test/`" or "name test files `FooTest.kt`" or "use `@RestController` for REST endpoints" adds nothing — Claude already knows standard conventions for every major language and framework. The litmus test: would Claude do this differently without the rule? If no, cut it.
- **Write tutorials.** A rule file is a constraint document, not a getting-started guide. State the rule, reference an example, move on.
- **Contradict existing rules.** Read existing `.claude/rules/` files and `CLAUDE.md` first. Do not introduce guidance that conflicts with what's already documented.
- **Skip `!IMPORTANT` markers.** Critical constraints that Claude might otherwise violate (e.g., "never use mocks", "only use kotlin.test assertions") MUST be marked with `!IMPORTANT`.
- **Exceed ~200 lines.** The file is consumed as context tokens. If you're over 200 lines, you're including too much. Cut tutorials, inline code, and obvious patterns. Aim for under 100 lines — the best rule files are 50-80 lines of high-signal constraints.

## Phase 3: Eval Loop

After writing the rule file, validate it by testing whether a fresh agent follows the conventions.

### Task Selection

- Pick a real file in the codebase that the rule applies to
- Choose a small task: write a test, add an endpoint, scaffold a screen
- Prefer files NOT used during discovery (avoids copying from exemplars)
- If the topic is too abstract for a concrete task, ask the user

### Run Eval

1. Launch a subagent with `isolation: "worktree"` and a concrete task. The subagent runs with full project context (CLAUDE.md + all rules). Do NOT inline the rule content — the point is to test whether the rule file works in-situ.

2. Review the subagent's output against each convention in the rule:
    - Does the code use the specified frameworks/libraries/annotations?
    - Does naming follow the documented conventions?
    - Are `!IMPORTANT` constraints respected?
    - Would the code work with the project's build system?

3. If all conventions followed: done.

4. If gaps found: determine whether the rule was unclear or the convention was missing. Update the rule file. Re-launch a fresh eval (new worktree).

5. Max 3 iterations. After 3, surface remaining issues to the user.

### Error Handling

- Worktree creation fails: skip eval, warn user, recommend manual review
- Subagent produces no output: count as failed iteration, retry with simpler task
- No suitable eval task: skip eval, inform user

## Phase 4: Completion

Tell the user: "Rule file written to `.claude/rules/<topic>.md`. Consider running `/claude-md-improver` (if available) when you're done making rule files to review the full setup."
