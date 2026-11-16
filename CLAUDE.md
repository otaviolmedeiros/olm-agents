# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A repo to version personal-assistant agents. There is no source code, build system, linter, or test suite: the content is Claude Code subagents written as Markdown files in `.claude/agents/` (frontmatter `name` + `description`, prompt in the body). Docs and agent prompts are in Brazilian Portuguese; keep that language when editing them.

Platform is Windows (PowerShell primary, Git Bash available). Branch `main`, remote `origin`.

## Agents and how they interact

- `secretaria`: Google Calendar scheduling/queries; reads the plan from Trello and calls `gerente-de-projetos` for reports; can email the user via Gmail. Read-only on the plan.
- `gerente-de-projetos`: owns the plan. Notion = objective tree/docs/Gantt, Trello = execution cards (title `[AAAA-MM-DD] ...`, labels `S-AAAA-Www` / `M-AAAA-MM`, fixed description format).

The Trello card convention is defined in `gerente-de-projetos.md` and duplicated in `secretaria.md` (which reads cards by it). When changing the convention, update both files.

`.claude/settings.local.json` is local permission config, not agent content.

When adding or renaming an agent, update the agents table in `README.md`.
