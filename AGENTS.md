# Repository instructions for Codex

## Project and sources of truth

- This repository is StreamTube. `docs/project-plan.md` defines product scope; `CHALLENGE.md` defines the Phase 03 acceptance criteria; existing code and decision records define established behavior.
- `AGENTS.md` is the Codex instruction entrypoint. The retained `CLAUDE.md` files are the original Claude Code source and remain useful for traceability, but use this file and nested `AGENTS.md` files as the active Codex guidance.
- Do not invent requirements or document behavior that is not supported by the plan, decisions, or code.

## Phase 03 workflow

Before implementing Phase 03, follow this pipeline in order:

1. `$research` -> record options, trade-offs, and recommendations in `docs/decisions/technical-decisions-phase-03-videos.md`.
2. `$plan-context` -> create `docs/phases/phase-03-videos/context.md`.
3. `$plan-validate` -> create `validation.md`; resolve all issues until its status is `clean`.
4. `$plan-resolve` -> materialize decisions and verify new libraries through Context7; create `library-refs.md` when applicable.
5. `$plan-build` -> create `phase-03-videos.md` with SI-03.x, all required Technical Specifications, Dependency Map, and Deliverables.
6. `$implement` -> execute the plan SI by SI, updating `progress.md` and advancing only after the current SI's checks pass.

Use `$plan-test-specs` only if test specifications are requested or the plan calls for them. Keep artifacts traceable to `docs/project-plan.md`, decisions, and code. The challenge requires the validation status to be clean before implementation.

## Using the ported Claude workflow in Codex

- Repository skills live under `.agents/skills/`. Invoke a skill with `$skill-name` (for example, `$research`), not a Claude slash command.
- The skill bodies were ported from `.claude/skills/`; supporting references and templates were copied with them. Read the relevant skill and its linked references before following a specialized workflow.
- Claude's `Read`, `Grep`, and `Glob` tool names mean bounded file reading and searching with the available filesystem tools, `rg`, and shell commands. Keep reads focused and preserve the procedures' output contracts.
- Use a direct user question when a workflow requires `AskUserQuestion`; do not invent the user's decisions.
- The six Claude reader agents are ported as read-only Codex custom agents in `.codex/agents/`. When a pipeline skill requests a named reader, delegate to the matching Codex role and pass its input contract. `$plan-readers` preserves the same procedures as a manual fallback if delegation is unavailable. Never claim a reader agent ran unless it actually did.
- Use the configured Context7 MCP server for official library documentation lookups when a relevant library is being evaluated or implemented.
- Figma MCP is optional and is documented in `.codex/config.toml.example`; use it only when that local service is running and a task needs Figma.

## Repository workflow and quality

- Work on feature branches created from `dev`; merge feature work back to `dev`. Never commit directly to `main`. Keep commits short and descriptive.
- Docker is the development environment. For container-to-container addresses use Compose service names, never `localhost`.
- Follow the nested `nestjs-project/AGENTS.md` or `next-frontend/AGENTS.md` for component-specific commands and conventions.
- For backend tests, use `*.spec.ts` for unit tests, `*.integration-spec.ts` for real database/service integration tests, and `*.e2e-spec.ts` for HTTP end-to-end tests.
- Before declaring Phase 03 complete, the relevant and full test suites must pass, `npx tsc --noEmit` must exit 0, and `npm run lint` must pass. Follow the backend container-only command rule in `nestjs-project/AGENTS.md`.
- Update the phase `progress.md` as work advances and update the Codex instructions after implementation so they match the actual code.

## General engineering principles

- Keep each module, service, and function focused on one responsibility. Keep types explicit and follow the installed TypeScript, ESLint, and Prettier conventions.
- Keep architecture, setup, and troubleshooting documentation aligned with the code. Consult the relevant official library documentation before using a library API, following its installed version.
- Work on one feature, fix, or refactor at a time. Do not mix cosmetic cleanup into a functional change; record out-of-scope work separately.
- For any planning, implementation, debugging, refactoring, or review request, identify and load the matching skill from `.agents/skills/` where one exists.
- Claude `Task` and subagent calls described in copied skills are not assumed to be available. Apply the procedure in the current thread unless an actual Codex delegation capability is exposed and useful; preserve each procedure's read/write scope and report truthfully which approach ran.
