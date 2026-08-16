# Initial Instruction to Codex

You are taking over implementation of the `trading-agentic-ai` repository after an architecture and planning phase completed with ChatGPT.

## First action

Before writing code:

1. Read `AGENTS.md` completely.
2. Read `PROJECT_HANDOFF_TO_CODEX.md` completely.
3. Inspect the current repository.
4. Treat the two Markdown files as the approved initial project context.

## Important

Do **not** redesign the architecture at this stage.

Do **not** assume that undocumented ideas from prior conversations are approved.

GitHub is now the shared context layer between ChatGPT, Codex, and later Claude review.

## Your role

You are the primary implementation engineer.

You will handle:

- coding,
- tests,
- debugging,
- refactoring,
- repository changes,
- implementation-linked documentation.

ChatGPT remains the architecture/planning layer.

Claude will later act as an independent focused reviewer of diffs/PRs, tests, edge cases, security, and architecture compliance.

## Context-efficiency rule

Do not require Claude to relearn the entire project for every review.

Prepare changes so Claude can review from:

- the relevant task context,
- the relevant architecture section,
- the Git diff/PR,
- test results,
- specific questions if needed.

## After reading the handoff

Respond with a concise project-ingestion report containing:

1. your understanding of the system architecture,
2. the key architecture boundaries you must preserve,
3. the phase sequence,
4. the role split between ChatGPT, Codex, Claude, and GitHub,
5. what you believe the first Phase 1 implementation slice should be,
6. any contradiction or ambiguity you find in the handoff.

Do not start large-scale implementation until this ingestion step is complete.
