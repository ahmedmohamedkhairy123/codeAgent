<!-- codeAgent:start -->
# codeAgent — Code Intelligence

This project is indexed by codeAgent as **codeAgent** (2273 symbols, 5419 relationships, 174 execution flows). Use the codeAgent MCP tools to understand code, assess impact, and navigate safely.

> If any codeAgent tool warns the index is stale, run `npx codeAgent analyze` in terminal first.

## Always Do

- **MUST run impact analysis before editing any symbol.** Before modifying a function, class, or method, run `codeAgent_impact({target: "symbolName", direction: "upstream"})` and report the blast radius (direct callers, affected processes, risk level) to the user.
- **MUST run `codeAgent_detect_changes()` before committing** to verify your changes only affect expected symbols and execution flows.
- **MUST warn the user** if impact analysis returns HIGH or CRITICAL risk before proceeding with edits.
- When exploring unfamiliar code, use `codeAgent_query({query: "concept"})` to find execution flows instead of grepping. It returns process-grouped results ranked by relevance.
- When you need full context on a specific symbol — callers, callees, which execution flows it participates in — use `codeAgent_context({name: "symbolName"})`.

## When Debugging

1. `codeAgent_query({query: "<error or symptom>"})` — find execution flows related to the issue
2. `codeAgent_context({name: "<suspect function>"})` — see all callers, callees, and process participation
3. `READ codeAgent://repo/codeAgent/process/{processName}` — trace the full execution flow step by step
4. For regressions: `codeAgent_detect_changes({scope: "compare", base_ref: "main"})` — see what your branch changed

## When Refactoring

- **Renaming**: MUST use `codeAgent_rename({symbol_name: "old", new_name: "new", dry_run: true})` first. Review the preview — graph edits are safe, text_search edits need manual review. Then run with `dry_run: false`.
- **Extracting/Splitting**: MUST run `codeAgent_context({name: "target"})` to see all incoming/outgoing refs, then `codeAgent_impact({target: "target", direction: "upstream"})` to find all external callers before moving code.
- After any refactor: run `codeAgent_detect_changes({scope: "all"})` to verify only expected files changed.

## Never Do

- NEVER edit a function, class, or method without first running `codeAgent_impact` on it.
- NEVER ignore HIGH or CRITICAL risk warnings from impact analysis.
- NEVER rename symbols with find-and-replace — use `codeAgent_rename` which understands the call graph.
- NEVER commit changes without running `codeAgent_detect_changes()` to check affected scope.

## Tools Quick Reference

| Tool | When to use | Command |
|------|-------------|---------|
| `query` | Find code by concept | `codeAgent_query({query: "auth validation"})` |
| `context` | 360-degree view of one symbol | `codeAgent_context({name: "validateUser"})` |
| `impact` | Blast radius before editing | `codeAgent_impact({target: "X", direction: "upstream"})` |
| `detect_changes` | Pre-commit scope check | `codeAgent_detect_changes({scope: "staged"})` |
| `rename` | Safe multi-file rename | `codeAgent_rename({symbol_name: "old", new_name: "new", dry_run: true})` |
| `cypher` | Custom graph queries | `codeAgent_cypher({query: "MATCH ..."})` |

## Impact Risk Levels

| Depth | Meaning | Action |
|-------|---------|--------|
| d=1 | WILL BREAK — direct callers/importers | MUST update these |
| d=2 | LIKELY AFFECTED — indirect deps | Should test |
| d=3 | MAY NEED TESTING — transitive | Test if critical path |

## Resources

| Resource | Use for |
|----------|---------|
| `codeAgent://repo/codeAgent/context` | Codebase overview, check index freshness |
| `codeAgent://repo/codeAgent/clusters` | All functional areas |
| `codeAgent://repo/codeAgent/processes` | All execution flows |
| `codeAgent://repo/codeAgent/process/{name}` | Step-by-step execution trace |

## Self-Check Before Finishing

Before completing any code modification task, verify:
1. `codeAgent_impact` was run for all modified symbols
2. No HIGH/CRITICAL risk warnings were ignored
3. `codeAgent_detect_changes()` confirms changes match expected scope
4. All d=1 (WILL BREAK) dependents were updated

## Keeping the Index Fresh

After committing code changes, the codeAgent index becomes stale. Re-run analyze to update it:

```bash
npx codeAgent analyze
```

If the index previously included embeddings, preserve them by adding `--embeddings`:

```bash
npx codeAgent analyze --embeddings
```

To check whether embeddings exist, inspect `.codeAgent/meta.json` — the `stats.embeddings` field shows the count (0 means no embeddings). **Running analyze without `--embeddings` will delete any previously generated embeddings.**

> Claude Code users: A PostToolUse hook handles this automatically after `git commit` and `git merge`.

## CLI

| Task | Read this skill file |
|------|---------------------|
| Understand architecture / "How does X work?" | `.claude/skills/codeAgent/codeAgent-exploring/SKILL.md` |
| Blast radius / "What breaks if I change X?" | `.claude/skills/codeAgent/codeAgent-impact-analysis/SKILL.md` |
| Trace bugs / "Why is X failing?" | `.claude/skills/codeAgent/codeAgent-debugging/SKILL.md` |
| Rename / extract / split / refactor | `.claude/skills/codeAgent/codeAgent-refactoring/SKILL.md` |
| Tools, resources, schema reference | `.claude/skills/codeAgent/codeAgent-guide/SKILL.md` |
| Index, status, clean, wiki CLI commands | `.claude/skills/codeAgent/codeAgent-cli/SKILL.md` |

<!-- codeAgent:end -->
