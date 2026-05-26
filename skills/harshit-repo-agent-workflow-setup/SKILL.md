---
name: harshit-repo-agent-workflow-setup
description: "Sets up Harshit's repo-scoped AI agent workflow: project-scoped skills.sh installs, AGENTS.md/CLAUDE.md, docs/agents, docs/features, CONTEXT.md, LEARNINGS.md, ISSUES.md, local markdown work tracking, context-first discovery, grilling, phased TDD, code review, logging, migration safety, and no build/dev command guardrails."
---

# Harshit Repo Agent Workflow Setup

Scaffold the per-repo configuration that Harshit's workflow assumes:

- **Project skills** — required skills installed repo-scoped with skills.sh, never global
- **Agent instructions** — one source of truth in `AGENTS.md`, with `CLAUDE.md` optionally symlinked
- **Local work tracking** — feature docs and phase breakdowns in `docs/features/`
- **Agent config docs** — `docs/agents/` tells skills where work, triage labels, and domain docs live
- **Memory files** — `CONTEXT.md`, `LEARNINGS.md`, `ISSUES.md`
- **Safety rails** — context-first discovery, grilling, phased TDD, smallest clear implementation, review, logging, migration safety, no build/dev commands

This is a prompt-driven setup skill, not a deterministic script. Explore, present what you found, make conservative recommendations, confirm risky replacements, then write.

## Non-Negotiables

- Install skills project-scoped only. Never use `-g`.
- Do not run build commands.
- Do not run dev servers.
- Do not overwrite existing user content blindly.
- Do not edit installed skill files.
- Prefer `AGENTS.md` as the source of truth.
- Preserve unique existing `CLAUDE.md` content before replacing it with a symlink.

## Process

### 1. Explore

Read the current folder before deciding what to create. Do not assume repo shape.

Check:

- `pwd`
- `ls -la`
- `git remote -v || true`
- existing `AGENTS.md`, `CLAUDE.md`, `CONTEXT.md`, `LEARNINGS.md`, `ISSUES.md`, `docs/`
- package/workspace markers: `package.json`, lockfiles, workspace files, monorepo config
- common code roots: `src/`, `app/`, `apps/`, `packages/`, `workers/`
- database markers: Prisma schema/migrations or other migration folders
- existing test/lint/typecheck scripts in package files

Classify the folder:

- **Empty folder** — no app or code files yet
- **Fresh starter** — minimal app/package files
- **Real codebase** — app code, tests, schema, routes, packages, or workers already exist

Infer project name from README/package/folder. If unclear, ask for the project name before writing `AGENTS.md`.

### 2. Present Findings And Defaults

Summarize what exists and what is missing. Then present recommended defaults:

- `AGENTS.md` as source of truth
- `CLAUDE.md` symlink to `AGENTS.md` when safe
- local markdown work tracker under `docs/features/`
- single-context domain docs unless the repo clearly needs `CONTEXT-MAP.md`
- default local triage labels: `needs-triage`, `needs-info`, `ready-for-agent`, `ready-for-human`, `wontfix`
- project-scoped skill installs only

Ask only for decisions that are risky or unclear, such as replacing existing `CLAUDE.md`, choosing project name, or preserving unusual existing docs.

### 3. Install Skills Project-Scoped

Use `npx skills` from the repo root. Never pass `-g`.

Install base workflow skills:

```bash
npx skills add mattpocock/skills --skill setup-matt-pocock-skills grill-with-docs to-prd to-issues diagnose prototype tdd triage zoom-out -y
```

Install or verify these project-scoped skills:

- `context7`
- `caveman-review`
- `test-driven-development` or `tdd`

If `context7` is missing, add it automatically without asking. Use `npx skills find context7` when the exact package source is unknown, then install the best exact match project-scoped from the repo root. Never skip Context7; the workflow depends on current library/framework/API docs.

Use `npx skills find <query>` when any other exact package source is unknown.

Install domain skills only when repo uses that domain:

- React/frontend: `frontend-design`, `web-design-guidelines`
- shadcn: `shadcn`
- TanStack: `tanstack-query-best-practices`, `tanstack-start-best-practices`, `tanstack-form`
- Prisma: `prisma-cli`, `prisma-client-api`, `prisma-database-setup`
- Better Auth: `better-auth-best-practices`
- Hono: `hono`
- Turborepo: `turborepo`
- Motion/animation: `ui-animation`, `framer-motion`

Verify with:

```bash
npx skills list --json
```

Important installed skills must show `"scope": "project"`.

### 4. Draft Files

Show the user a concise draft before writing if existing files will be replaced or heavily changed.

Create or update these files:

- `AGENTS.md`
- `CLAUDE.md` symlink, when safe
- `CONTEXT.md`
- `LEARNINGS.md`
- `ISSUES.md`
- `docs/README.md`
- `docs/features/README.md`
- `docs/agents/issue-tracker.md`
- `docs/agents/triage-labels.md`
- `docs/agents/domain.md`

Do not create old-style planning/spec trees. Do not create `.scratch/` unless user explicitly asks.

### 5. AGENTS.md Content

Keep `AGENTS.md` concise. Include these rules, adapted to the repo:

- project name at top
- code is source of truth; docs do not override code
- use `CONTEXT.md` for product/domain language
- do not rely on memory when facts matter
- gather context before `grill-with-docs`
- context gathering includes docs, code search, touchpoint mapping, Context7, web search/fetch when needed
- invoke domain skills when that domain work starts, not all upfront
- after grilling, use `to-prd` for readable feature doc in `docs/features/`
- use `to-issues` for vertical phase breakdown inside same feature doc
- local markdown work tracking only unless user asks otherwise
- never run build commands or dev servers
- small-change exception for typo/copy/tiny styling/one-line obvious bugs
- unrelated issues go to `ISSUES.md`
- prefer the smallest clear implementation without clever compression
- TDD per feature phase
- new features and changed error paths need structured, searchable logs
- migration safety: schema changes need migrations; never edit existing migrations
- always invoke `caveman-review` for code review
- keep docs small

Use package-manager examples that match the repo. For unknown package manager, list generic forbidden categories instead of specific commands.

Include a short `Code Size And Readability` section:

- prefer the smallest clear implementation
- keep feature code readable enough for the user to review directly
- avoid clever compression, broad abstractions, and framework-heavy patterns just to reduce line count
- use less code only when it makes behavior easier to understand
- use more code when explicit steps make product logic clearer
- add helpers, abstractions, or shared modules only when they remove real duplication or complexity

### 6. Supporting Docs Content

`docs/features/README.md` should say feature docs are plain English and include:

- what we are building
- decisions from grill
- what we are not building
- code touchpoints
- phases from `to-issues`
- what user should check in each phase
- tests and verification
- logging and error tracking
- open questions

`docs/agents/issue-tracker.md` should configure local markdown:

- work lives in `docs/features/<feature-slug>.md`
- `to-prd` writes feature docs locally
- `to-issues` writes phase breakdown inside same doc
- no external tracker publishing unless user asks

`docs/agents/triage-labels.md` should define:

- `needs-triage`
- `needs-info`
- `ready-for-agent`
- `ready-for-human`
- `wontfix`

`docs/agents/domain.md` should say:

- read `CONTEXT.md`
- read `LEARNINGS.md`
- read `docs/README.md`
- use `docs/features/` for active/recent feature work
- use `docs/adr/` only if present
- create `CONTEXT-MAP.md` only when the repo has real multiple domain contexts

`LEARNINGS.md` should stay short: recurring repo lessons only.

`ISSUES.md` should track unrelated bugs, mismatches, cleanup, and risks found during work. Include fields:

- Found while
- Area
- Problem
- Impact
- Evidence
- Suggested next step
- Status

### 7. Handle Existing Files

If `AGENTS.md` exists, update it in place. Preserve user-specific rules.

If `CLAUDE.md` exists:

1. Read it.
2. Extract unique repo-critical rules.
3. Merge those rules into `AGENTS.md`.
4. Replace `CLAUDE.md` with symlink only after preserving content or confirming with user.

Preferred symlink:

```bash
ln -s AGENTS.md CLAUDE.md
```

If symlink is not appropriate, keep both files aligned manually.

### 8. Final Checks

Run static checks only:

- list created files
- confirm `CLAUDE.md` symlink if applicable
- confirm project-scoped skills with `npx skills list --json`
- scan active docs for accidental forbidden commands or external-tracker defaults

Do not run build or dev server.

### 9. Done

Report:

- installed project skills
- files created/updated
- whether `CLAUDE.md` was symlinked
- existing docs preserved or merged
- unresolved choices
- verification run

Mention: newly installed skills may require restarting the agent session before they appear in the available skill list.
