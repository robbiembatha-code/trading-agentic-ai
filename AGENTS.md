# Trading Agentic AI — Codex Operating Instructions

Before making implementation changes in this repository, read `PROJECT_HANDOFF_TO_CODEX.md` in full.

That document is the approved initial project context transferred from the ChatGPT architecture and planning stage.

## Source of truth

For this initial stage, treat `PROJECT_HANDOFF_TO_CODEX.md` as the authoritative handoff.

Do not assume that every idea from prior conversations is approved. Only decisions recorded in the repository should be treated as project decisions.

## Architecture rule

Do not redesign the system unless explicitly instructed.

Preserve clear separation between:

1. market-data ingestion and normalization,
2. feature engineering and market-structure analysis,
3. strategy/signal generation,
4. AI reasoning,
5. deterministic risk and position sizing,
6. execution and trade routing,
7. multi-account orchestration,
8. monitoring, logging, and audit.

The AI reasoning layer must never bypass deterministic risk controls.

## Trading safety rule

No model or agent may directly override:

- hard risk limits,
- position-sizing controls,
- stop-loss requirements,
- account exposure limits,
- drawdown limits,
- broker/execution constraints,
- emergency shutdown controls.

## Current stage

The project is transitioning from architecture/planning into implementation.

No production implementation should be assumed to exist unless it is present in the repository.

Codex should first understand the handoff and repository, then continue from the approved phase sequence and current next step.

## Collaboration model

- **ChatGPT**: architecture, planning, requirements, decomposition, strategy design, and architectural decisions.
- **Codex**: implementation, coding, tests, debugging, refactoring, and repository changes.
- **Claude**: focused independent review of code, diffs, tests, security, edge cases, and architecture compliance.
- **GitHub**: persistent shared context and source of truth between agents.

Claude should receive focused context: relevant architecture, the task specification, and the Git diff/PR. It should not repeatedly ingest the entire project history.

## Completion behavior

For each implementation task, Codex should:

1. read the relevant repository context,
2. implement only the requested scope,
3. add or update tests,
4. preserve architecture boundaries,
5. document material assumptions or deviations,
6. produce a clean commit/PR suitable for Claude review,
7. avoid expanding scope unless a dependency is genuinely required.
