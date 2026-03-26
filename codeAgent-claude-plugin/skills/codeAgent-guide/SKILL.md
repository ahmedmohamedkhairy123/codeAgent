---
name: codeAgent-guide
description: "Use when the user asks about codeAgent itself — available tools, how to query the knowledge graph, MCP resources, graph schema, or workflow reference. Examples: \"What codeAgent tools are available?\", \"How do I use codeAgent?\""
---

# codeAgent Guide

Quick reference for all codeAgent MCP tools, resources, and the knowledge graph schema.

## Always Start Here

For any task involving code understanding, debugging, impact analysis, or refactoring:

1. **Read `codeAgent://repo/{name}/context`** — codebase overview + check index freshness
2. **Match your task to a skill below** and **read that skill file**
3. **Follow the skill's workflow and checklist**

> If step 1 warns the index is stale, run `npx codeAgent analyze` in the terminal first.

## Skills

| Task                                         | Skill to read       |
| -------------------------------------------- | ------------------- |
| Understand architecture / "How does X work?" | `codeAgent-exploring`         |
| Blast radius / "What breaks if I change X?"  | `codeAgent-impact-analysis`   |
| Trace bugs / "Why is X failing?"             | `codeAgent-debugging`         |
| Rename / extract / split / refactor          | `codeAgent-refactoring`       |
| Tools, resources, schema reference           | `codeAgent-guide` (this file) |
| Index, status, clean, wiki CLI commands      | `codeAgent-cli`               |

## Tools Reference

| Tool             | What it gives you                                                        |
| ---------------- | ------------------------------------------------------------------------ |
| `query`          | Process-grouped code intelligence — execution flows related to a concept |
| `context`        | 360-degree symbol view — categorized refs, processes it participates in  |
| `impact`         | Symbol blast radius — what breaks at depth 1/2/3 with confidence         |
| `detect_changes` | Git-diff impact — what do your current changes affect                    |
| `rename`         | Multi-file coordinated rename with confidence-tagged edits               |
| `cypher`         | Raw graph queries (read `codeAgent://repo/{name}/schema` first)           |
| `list_repos`     | Discover indexed repos                                                   |

## Resources Reference

Lightweight reads (~100-500 tokens) for navigation:

| Resource                                       | Content                                   |
| ---------------------------------------------- | ----------------------------------------- |
| `codeAgent://repo/{name}/context`               | Stats, staleness check                    |
| `codeAgent://repo/{name}/clusters`              | All functional areas with cohesion scores |
| `codeAgent://repo/{name}/cluster/{clusterName}` | Area members                              |
| `codeAgent://repo/{name}/processes`             | All execution flows                       |
| `codeAgent://repo/{name}/process/{processName}` | Step-by-step trace                        |
| `codeAgent://repo/{name}/schema`                | Graph schema for Cypher                   |

## Graph Schema

**Nodes:** File, Function, Class, Interface, Method, Community, Process
**Edges (via CodeRelation.type):** CALLS, IMPORTS, EXTENDS, IMPLEMENTS, DEFINES, MEMBER_OF, STEP_IN_PROCESS

```cypher
MATCH (caller)-[:CodeRelation {type: 'CALLS'}]->(f:Function {name: "myFunc"})
RETURN caller.name, caller.filePath
```
