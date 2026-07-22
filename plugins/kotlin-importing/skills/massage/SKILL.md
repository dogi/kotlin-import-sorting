---
name: massage
description: Sort Kotlin import blocks, remove unused imports, and normalize blank lines around imports in any Kotlin/Android project. Use when asked to clean up, sort, or organize imports in .kt files, or to remove unused imports without running ktlint.
---

# Kotlin import cleanup

Sorts imports, removes unused imports, and normalizes blank lines across Kotlin
files — a safe, offline alternative to ktlint's `import-ordering` +
`no-unused-imports` rules. Works on any Kotlin project.

## What it does

For every `*.kt` file under the given roots:

1. **Sorts** the import block alphabetically by import path (case-sensitive
   ASCII order — matches ktlint's default, so it won't fight the linter).
2. **Removes unused imports** — an import is dropped only if its simple name
   never appears anywhere else in the file.
3. **Normalizes blank lines** — exactly one blank line before and after the
   import block, and no blank lines inside it.

## How to run

The `kotlin-importing.py` script is bundled next to this SKILL.md inside the plugin.
Locate it in this skill's directory (`${CLAUDE_PLUGIN_ROOT}/skills/massage/kotlin-importing.py`)
and run it against the project's Kotlin source root(s):

```bash
# Whole project (current directory)
python3 <skill-dir>/kotlin-importing.py .

# A specific source root
python3 <skill-dir>/kotlin-importing.py app/src src/main/kotlin

# Preview without writing (report only)
python3 <skill-dir>/kotlin-importing.py --check app/src
```

It prints how many files changed, every unused import removed, and any wildcard
imports found. It only rewrites files whose content changes, so it is safe to
re-run (idempotent).

## Safety model (why removals don't break the build)

Unused-import detection only ever **over-keeps**, never over-removes:

- **Backticks are stripped** before matching, so escaped identifiers like the
  Mockito `` `when` `` are detected as used.
- **A dot counts as a word boundary**, so extension-function calls (`.map`,
  `.collect`, `.launchIn`) count as usage of the imported name.
- **Operator / convention functions are never auto-removed** (`plus`, `get`,
  `getValue`, `setValue`, `provideDelegate`, `component1..N`, `contains`, ...)
  since they can be invoked via operator syntax without the name appearing.

## Wildcard imports

Wildcard (`import x.*`) imports are **reported** but **not expanded** — reliable
expansion needs compiler symbol resolution. Expand those by hand or via the
IDE's "Optimize Imports".

## After running

Review `git diff` and let the project's build/CI confirm correctness.
