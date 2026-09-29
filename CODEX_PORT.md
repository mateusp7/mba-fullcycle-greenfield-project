# Claude Code to Codex port

The Claude setup remains intact under `.claude/` and `CLAUDE.md`. Codex uses the parallel project-native structure:

| Claude Code source | Codex port |
| --- | --- |
| Root `CLAUDE.md` | Root `AGENTS.md` |
| Backend `nestjs-project/CLAUDE.md` | `nestjs-project/AGENTS.md` |
| Frontend `next-frontend/CLAUDE.md` | `next-frontend/AGENTS.md` |
| `.claude/skills/` | `.agents/skills/` (supporting references and assets included) |
| `.claude/rules/` | `.agents/rules/` (consulted by path from nested `AGENTS.md`) |
| `.claude/agents/*.md` | `.codex/agents/*.toml` read-only Codex custom agents; original procedure copies remain in `.agents/skills/plan-readers/references/` |
| `.mcp.json` | `.codex/config.toml` |
| Claude settings hook | Root `AGENTS.md` skill-selection guidance; no project hook was added |

Codex skills use `$skill-name`. Claude-only frontmatter fields were removed from the copied skill manifests. The six reader agents are registered as project-scoped Codex custom agents with read-only sandbox settings. Their original instructions are wrapped in Codex TOML roles; the reader skill keeps a manual fallback. Other generic Claude `Task` calls without a named role are mapped in `AGENTS.md`. All skill content and supporting resources are preserved.

The Codex MCP config defines the existing PostgreSQL stdio server and Context7's remote MCP endpoint used by the challenge's library research. The optional Figma MCP URL from `.mcp.json.example` is shown in `.codex/config.toml.example` and requires its local Figma service to be running.
