# BigQuery Graph Authoring Skill (Preview)

A skill that turns a BigQuery dataset — and, when available, the customer's own
model documents (data dictionaries, ER diagrams, rule notes) — into a property
graph with semantic definitions (dimensions and measures), with every candidate
relationship verified against the data before it lands.

**This is a preview release.** The official version will ship with the
[Data Agent Kit](https://github.com/gemini-cli-extensions/data-agent-kit-starter-pack/blob/main/skills/bigquery-graph/SKILL.md)
and will be available on Sep 7.

## What's inside

```
bigquery-graph/
  SKILL.md                 # entry point: stations index + core rules
  references/              # per-station playbooks, loaded on demand
    graph-schema/          # discovery, document handling, verification,
                           # DDL reference, semantic enrichment, ...
    graph_queries.md
    semantic_queries.md
```

## Usage

This is a **pure instruction skill — it ships no tools**. The host agent brings
its own capabilities; the skill only assumes:

- a way to execute BigQuery SQL (any SQL-execution tool, or the platform's
  own query surface — the references anchor every step in
  `INFORMATION_SCHEMA` queries rather than named tools);
- the contents of `references/` available to it in some form — a skill-resource
  loader, a file-reading tool, or simply pre-loading the files into context
  all work; the skill does not assume any particular mechanism.

To install, mount the `bigquery-graph/` directory into any harness that loads
skills by directory (the directory name must match the `name:` field in the
SKILL.md frontmatter).

## What it does

Discovery over `INFORMATION_SCHEMA` and job history → optional document
extraction (text or diagrams) → relationship verification against the data
(every edge needs measured evidence, never a document's word alone) → a plan
the user approves → `CREATE PROPERTY GRAPH` DDL, shown before it runs → an
opt-in verification pass on the landed graph.
