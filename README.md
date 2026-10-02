# agentic-coding-template

Generic, reusable agentic-engineering setup for a code authoring agent: skills, agent
definitions, instructions (AGENTS.md), and CI + git watch/restart scripts — scrubbed of
all internal/infrastructure references. Copy it into your own projects and adapt.

## Contents map

| Path | What it is |
|------|------------|
| `AGENTS.md` | Instructions template — fill the `<PLACEHOLDER>` tokens, then adapt + trim |
| `skills/` | Authoring-skills kit, most generic (TDD workplan, long-drill discipline, modular design/planner/reviewer, commands) |
| `skills/hotpath-mcp.md` | **Tool-specific** skill example (real public Rust profiling library with an MCP server) — shows the shape for language/tool-specific additions; delete or adapt if it does not apply |
| `agents/` | Subagent definitions: `code-verifier` (read-only verification), `git-ops` (commit/push only), `ci-watcher` (watch GitHub Actions runs, report-only) |
| `scripts/` | `watch-ci.sh` (watch a GitHub Actions run to terminal state) and `watch-git.sh` (observe a detached commit/push drive, restart when stuck/failed) |

## How to use

1. Copy this tree into a new project.
2. Fill the `<PLACEHOLDER>` tokens in `AGENTS.md`, then adapt + trim it for your project.
3. Drop `skills/` into your project's `.agent/skills/` (or your agent tool's skills dir).
4. Drop `agents/` into the opencode agents dir — `.opencode/agents/` in-project, or global `~/.config/opencode/agents/` (see your agent tool's docs). `chmod +x scripts/*.sh`.
5. Adapt/trim as needed — this is a starting kit, not a contract.

## Flagship patterns

- **Watch CI and react** — `scripts/watch-ci.sh` plus the `ci-watcher` agent: check a
  workflow run to terminal state, classify phases (`queued` → `in_progress` →
  `success`/`failure`/`cancelled`/...), report verdicts with verbatim evidence. Deadlines
  on every wait, machine-readable `STATE|RUN|...` / `FINAL|VERDICT|...` output.
- **Observe commit/push, restart when stuck** — `scripts/watch-git.sh`: monitor a
  detached drive script using the sentinel protocol (`<loop>-<N>.log`, `<loop>-<N>.done`,
  `<loop>.final` = landed HEAD hash or `FAILED`), and re-launch it detached when stuck or
  failed — bounded restarts, hard deadlines, `/var/tmp` scratch guidance.

## Notes

- This is the generic, infra-free extraction. Consumers add their own language/tool-specific
  agents (coder/reviewer pairs, etc.) by example of the included tool-specific skill
  (`hotpath-mcp.md`).
- All watchers follow long-drill discipline (MILE UTC heartbeats, deadlines, machine-readable
  verdicts) — see `skills/long-drill.md` for the full doctrine.