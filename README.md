# Harshit Skills

Repo-scoped agent workflow skills for building software with fewer AI mistakes.

The main idea is simple: the codebase is the source of truth, docs stay small, agents gather context before asking questions, and feature work happens in small reviewed phases.

## Quickstart

Run this from the repo where you want the workflow installed:

```bash
npx skills@latest add C-W-D-Harshit/harshit-skills --skill harshit-repo-agent-workflow-setup
```

Then ask your agent:

```text
Use harshit-repo-agent-workflow-setup
```

The setup skill will inspect the project first, then create or update the repo-scoped agent files.

## What It Sets Up

- `AGENTS.md` as the main agent instruction file
- optional `CLAUDE.md` symlink to `AGENTS.md`
- `CONTEXT.md` for product/domain language
- `LEARNINGS.md` for recurring repo lessons
- `ISSUES.md` for unrelated bugs found during feature work
- `docs/features/` for readable feature docs and phase breakdowns
- `docs/agents/` for local tracker, triage, and domain-doc configuration
- project-scoped skills.sh installs, not global installs
- context-first discovery before grilling
- feature docs, vertical phases, TDD, and code review
- smallest clear implementation guidance
- structured logging expectations
- migration safety rules
- no build command and no dev server guardrails

## Workflow

For feature work, the agent should:

1. Read existing docs and inspect the codebase.
2. Map the likely code touchpoints.
3. Check current library/framework docs when needed.
4. Use `grill-with-docs` only after the context pass.
5. Write a short readable feature doc.
6. Break the work into vertical phases.
7. Use TDD for each phase.
8. Review each phase before moving on.

This keeps planning grounded in the real code instead of stale documentation or model memory.

## Skills

| Skill | Purpose |
| --- | --- |
| `harshit-repo-agent-workflow-setup` | Installs Harshit's repo-scoped AI-agent workflow into an empty folder, fresh starter, or existing codebase. |

## Design Principles

- Code is the source of truth.
- Docs explain decisions; they do not override code.
- Prefer local markdown over external trackers unless explicitly requested.
- Ask fewer, better questions after reading the project.
- Invoke skills when the work reaches that domain, not all upfront.
- Keep docs readable for humans.
- Prefer the smallest clear implementation.
- Work in small vertical slices.
- Log failures so they are searchable later.
- Do not hide unrelated bugs in chat; capture them in `ISSUES.md`.

If the skill does not appear immediately in your agent, restart the agent session.

## License

MIT
