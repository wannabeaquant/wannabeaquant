# Weekly Pattern Review — 2026-05-31

## Repos Analyzed This Week
| Repo | Commits | Format Score | Status |
|------|---------|--------------|--------|
| wannabeaquant/Aperture | 2 | 5/5 | Active |
| wannabeaquant/aso-audit-agent | 3 | 5/5 | Active — NEW, no CLAUDE.md |
| wannabeaquant/pr-review-context | 14 | 3/5 | Active — NEW, no CLAUDE.md |
| wannabeaquant/Bliss | 1 | 5/5 | Active |
| wannabeaquant/DHARA | 1 | 5/5 | Active |
| wannabeaquant/Elsa-Hyperthon | 1 | 5/5 | Active |
| wannabeaquant/GhostLAN | 1 | 5/5 | Active |
| wannabeaquant/FactForager | 1 | 5/5 | Active |
| wannabeaquant/Portfolio | 1 | 5/5 | Active — NEW, no CLAUDE.md |
| wannabeaquant/ATLAS-v2 | 1 | 5/5 | Active |
| wannabeaquant/Polymarket-Bot | 1 | 5/5 | Active |

## New Projects Detected
- **wannabeaquant/aso-audit-agent** — Mastra + Claude ASO audit agent. Has README but no CLAUDE.md. Needs setup.
- **wannabeaquant/pr-review-context** — Hybrid LLM context retrieval for PR review. Has README + DESIGN.md but no CLAUDE.md. Most active repo this week — needs CLAUDE.md urgently.
- **wannabeaquant/Portfolio** — Only one automated review commit. No CLAUDE.md, no README visible. Needs setup or note that it's a static site.

## Work Summary
- **pr-review-context**: Week's heaviest work. Shipped LLM-based router (Haiku + heuristic fallback), diversity-aware greedy knapsack ranker, name-based git history that survives line shifts, indirect callee detection via `partial()`/`wraps()`, agent scratchpad tool, extended eval script covering 10 additional PR types. Heavy docs pass: trimmed DESIGN.md, added EVAL.md, fixed architecture diagrams, synced README.
- **aso-audit-agent**: New project built end-to-end — ASO audit agent with Mastra framework and Claude. Two follow-up fixes: correct Claude model ID (`claude-sonnet-4-5`) and API alignment to `@mastra/core@1.37` + `ai@6`.
- **Aperture**: Only automated weekly session docs commit. No code shipped.
- All other repos (Bliss, DHARA, Elsa-Hyperthon, GhostLAN, FactForager, Portfolio, ATLAS-v2, Polymarket-Bot): Single automated `docs(review)` commit each — dormant this week.

## Commit Health
- **8 of 11 repos have exactly 1 commit** — all automated `docs(review): weekly session notes 2026-05-24`. These are not real work commits; they represent zero active development this week.
- **pr-review-context format score 3/5**: Two issues. First, `eval` is used as a commit type (`eval: add extended eval script`) — not in the standard type list (`feat|fix|refactor|test|docs|chore|perf`). `test` or `chore` would be correct. Second, two commits are missing the `(scope)` component: `fix: three bugs found in external review` and `docs: fix architecture diagrams`. Format degrades under doc/polish sprints.
- **aso-audit-agent**: Two `fix` commits immediately after `feat` initial commit — suggests initial build had compatibility issues not caught before first push. Not a commit format problem but a ship-quality signal.
- **No repos have broken commit format on feat/fix/refactor types** — format discipline holds on code commits, only slips on docs/eval/chore.

## Patterns Observed This Week
- **Work concentrates into 1-2 repos per week while the rest go dormant.** 9 repos had zero real commits. This is expected for a student running parallel projects, but means MEMORY.md "Next Session Priorities" fields are consistently empty — they're not being used.
- **New repos get built quickly but CLAUDE.md setup lags.** Both `aso-audit-agent` and `pr-review-context` were built and committed this week without CLAUDE.md. The `/init`-based setup from global CLAUDE.md instructions is not being triggered consistently.
- **Docs commits accumulate on active features.** pr-review-context had 6 docs commits out of 14 — DESIGN.md trim, EVAL.md extraction, README sync, diagram fixes. This is good documentation practice but the commits often lack scope: `docs: fix architecture diagrams` instead of `docs(router): fix architecture diagrams`.
- **Model ID bugs on new Claude projects.** `aso-audit-agent` needed a fix commit specifically to correct the Claude model ID. This is a recurring new-project hazard when using Mastra or other wrappers that don't validate against the Anthropic API at config time.
- **eval as a commit type is non-standard and used inconsistently.** Only appeared in pr-review-context. Eval scripts are test-adjacent — `test(eval)` is the correct type.

## Recurring Issues
- **Non-standard commit types under pressure.** `eval` appeared this week; prior PROFILE.md noted format degrades during "fast visual/UI iteration sprints." Now confirmed it also degrades during doc/eval sprints. The failure mode is dropping `(scope)` and using ad-hoc types, not malformed messages.
- **New repos missing CLAUDE.md at build time.** This is the second or third occurrence across sessions (PROFILE.md already notes this). The global rule says to run `/init` on new repos, but the pattern is: build first, set up context files later (or never).

## Proposed Global CLAUDE.md Additions
```
## New Repo CLAUDE.md Trigger
- Any session that produces a first commit to a repo with no CLAUDE.md: stop before pushing and run /init (existing codebase) or hand-write CLAUDE.md (greenfield). Do not push initial code without CLAUDE.md in the same commit batch. # evidence: aso-audit-agent, pr-review-context, Portfolio all shipped this week without CLAUDE.md

## Commit Type: eval is not valid
- Use `test(eval): ...` for evaluation scripts, benchmarks, and scoring runs. Never use `eval` as a standalone type. # evidence: pr-review-context 2026-05-31
```

## Proposed Project-Level CLAUDE.md Updates

### wannabeaquant/pr-review-context
```
# pr-review-context

## What This Is
Hybrid context retrieval for LLM-powered PR review. Given a PR diff and a repo checkout, determines what additional context (callees, callers, tests, git history, type defs) to fetch while staying within a token budget.

## Stack
- Python 3.x
- Claude (Haiku for routing, Sonnet for agent path)
- No external database — file-based repo analysis

## Architecture
```
PR diff → Router (Haiku LLM + heuristic fallback)
       → Fast Path: all retrievers → rank → pack
       → Agent Path: Sonnet loop with 10 tools + scratchpad
       → JSON retrieval plan + review prompt
```

## Key Files
- Router: LLM-based routing via Haiku with heuristic fallback
- Ranker: diversity-aware greedy knapsack (prevents single-file budget saturation)
- Git history: name-based log (survives line number shifts + intra-file moves)
- Callees: detects indirect callees via partial()/wraps() as first-arg names
- Agent scratchpad: explicit reasoning thread tracking tool
- EVAL.md: evaluation methodology and PR type coverage
- DESIGN.md: architecture (~2 pages, canonical reference)

## Commit Conventions
- Use `test(eval): ...` for evaluation scripts — not `eval:` as a standalone type
- Always include `(scope)` in docs commits: `docs(router): ...` not `docs: ...`

## GitHub
https://github.com/wannabeaquant/pr-review-context

## Git & Commits
- Format: `<type>(<scope>): <short imperative description>`
- Every self-contained working change = one commit. Push immediately.

## Session Memory
- Read MEMORY.md at session start.
- On "session end": write summary to MEMORY.md.

## Error Log
- Read ERRORS.md before suggesting approaches.
- Log failures after 2+ attempts to ERRORS.md.
```

### wannabeaquant/aso-audit-agent
```
# aso-audit-agent

## What This Is
AI-powered App Store Optimization audit agent. Paste any App Store URL → comprehensive audit with weighted scores, competitor benchmarking, quick wins, and before/after recommendations.

## Stack
- TypeScript / Node.js
- Mastra framework (`@mastra/core@1.37`)
- Claude (`claude-sonnet-4-5` — exact model ID; do NOT use shorthand)
- `ai@6` SDK
- iTunes Search API (no auth)

## Commands
```bash
cp .env.example .env   # add ANTHROPIC_API_KEY
npm install
npm run dev            # http://localhost:4111
```

## Critical Facts
- Model ID is `claude-sonnet-4-5` — this burned a fix commit on initial build. Always verify against Anthropic API docs before wiring a new model string into Mastra config.
- Mastra API surface changes frequently — pin `@mastra/core` and `ai` versions and check compat on upgrade.

## GitHub
https://github.com/wannabeaquant/aso-audit-agent

## Git & Commits
- Format: `<type>(<scope>): <short imperative description>`
- Every self-contained working change = one commit. Push immediately.

## Session Memory
- Read MEMORY.md at session start.
- On "session end": write summary to MEMORY.md.

## Error Log
- Read ERRORS.md before suggesting approaches.
- Log failures after 2+ attempts to ERRORS.md.
```

## MEMORY.md Entries Suggested
- **pr-review-context**: No MEMORY.md exists yet — needs creation. Key decisions to log: (1) LLM routing via Haiku with heuristic fallback chosen over pure heuristic; (2) diversity-aware greedy knapsack over simple top-k ranking to prevent single-file saturation; (3) name-based git log over line-anchor approach for stability across rebases.
- **aso-audit-agent**: No MEMORY.md exists yet — needs creation. Decision to log: Mastra + Claude chosen as framework stack for ASO agent; `@mastra/core@1.37` + `ai@6` are the pinned compatible versions.
- All other repos: No new decisions made this week — MEMORY.md entries not warranted.

## Proposed PROFILE.md Additions
```
- Format discipline degrades not just during frontend sprints but also during doc/polish/eval sprints — `(scope)` gets dropped from docs commits and ad-hoc types like `eval` appear. Flag this at the start of any session that is primarily docs or evaluation work, not just UI work. # confirmed 2026-05-31 in pr-review-context
- New repos get built in a single session (consistent with greenfield pattern) but CLAUDE.md setup is deferred or skipped. This has now occurred 3+ times. The build instinct is strong; the context-file instinct is weak. Auto-trigger CLAUDE.md setup check at first commit of any new repo.
```

## Next Week Watch List
- **pr-review-context CLAUDE.md + MEMORY.md**: Most active repo this week ships without context files. If another session starts here without them, the pattern is entrenched — prioritize setup at next session open.
- **aso-audit-agent stability**: Two fix commits immediately after initial build suggests Mastra API surface is fragile. Watch for a third fix on next touch — if it happens, log the specific API gotchas to ERRORS.md.
- **Dormant project reactivation**: ATLAS-v2, Polymarket-Bot, and Bliss all have populated MEMORY.md/ERRORS.md but zero code commits this week. If any reactivate, verify MEMORY.md "Next Session Priorities" was written — those fields are currently blank across all three, which defeats the purpose.