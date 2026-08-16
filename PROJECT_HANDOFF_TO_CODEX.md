# Trading Agentic AI — Initial Project Handoff to Codex

## 1. Purpose of this document

This file transfers the approved project context created during the architecture, planning, phasing, and AI-collaboration stage into the GitHub repository so Codex can continue implementation without requiring the original ChatGPT conversation history.

This is an **initial handoff**, not a permanent giant context file. From this point onward, the repository should increasingly become the project source of truth.

---

## 2. Project objective

Build an agentic AI trading platform that can:

- ingest and normalize market data,
- detect market structure and price-action conditions,
- identify impulse/correction and displacement behavior,
- reason about trade setups using an AI reasoning layer,
- enforce deterministic risk and position-sizing rules,
- route approved trades to MT5 execution,
- scale toward multi-account execution,
- retain complete logs and auditability,
- support research, backtesting, model improvement, and future training workflows.

The initial trading focus is strongly associated with XAUUSD/gold, while the architecture should remain extensible to additional instruments and providers.

---

## 3. Core architectural principle

The system must be modular.

The principal flow is:

`Market Data -> Normalization -> Feature/Structure Analysis -> Signal/Setup Detection -> AI Reasoning -> Deterministic Risk Engine -> Trade Router -> MT5 Execution -> Monitoring/Audit`

The system should preserve strict boundaries between reasoning and execution.

The AI may evaluate and rank trade opportunities, but hard safety controls must remain deterministic and outside model discretion.

---

## 4. Major system layers

### 4.1 Market-data ingestion

Responsibilities:

- ingest market data from approved providers,
- support provider adapters,
- normalize symbols and timestamps,
- produce canonical market-data structures,
- detect connection/data-health issues,
- retain sufficient raw data for debugging and audit.

The architecture should be provider-neutral where practical.

Potential sources discussed include TradingView-compatible feeds, broker/MT5 data, and future institutional/market-data sources.

### 4.2 Data normalization and feature layer

Responsibilities include:

- OHLCV/candle normalization,
- timeframe handling,
- volatility measures,
- ATR-based context,
- swing/high-low structure,
- displacement metrics,
- impulse/correction characteristics,
- liquidity-related features,
- feature preparation for downstream models.

### 4.3 Market-structure and setup detection

The project is intended to formalize trading logic rather than rely on free-form AI judgment alone.

Important concepts discussed include:

- market structure,
- impulse and correction,
- displacement detection,
- liquidity sweeps,
- structural breaks/continuations,
- volatility context,
- confluence-driven setup identification.

These concepts should become deterministic or well-defined analytical components wherever possible before being passed into the reasoning layer.

### 4.4 AI reasoning layer

The reasoning layer interprets structured market context and setup candidates.

Responsibilities can include:

- evaluating setup quality,
- combining multiple structured signals,
- reasoning over current market context,
- generating an explainable trade thesis,
- ranking candidate opportunities,
- producing a proposed trade intent.

The reasoning layer should not directly place trades and should not determine final risk outside approved constraints.

### 4.5 Deterministic risk engine

This is a hard control layer and has final authority before execution.

Expected responsibilities include:

- position sizing,
- maximum risk per trade,
- stop-loss validation,
- account-level exposure,
- correlated exposure,
- daily/weekly drawdown limits,
- maximum simultaneous positions,
- trade-frequency constraints where required,
- broker constraints,
- kill switch / emergency shutdown controls.

A model output that violates the risk engine must be rejected or resized according to explicit rules.

### 4.6 Execution and trade routing

The execution layer receives only risk-approved trade instructions.

Responsibilities include:

- translate normalized trade intent into broker/MT5 orders,
- handle execution errors and retries safely,
- track order status,
- detect rejected/partial/failed orders,
- prevent duplicate execution,
- reconcile expected and actual positions,
- maintain execution logs.

### 4.7 MT5 integration

MT5 is part of the intended execution environment.

The system direction includes:

- Python/backend services for analysis and orchestration,
- an MT5 Expert Advisor or execution bridge,
- authenticated communication between backend and terminal,
- separation of signal/risk logic from terminal execution logic.

The EA/bridge should act primarily as an execution interface, not as the owner of AI reasoning.

### 4.8 Multi-account orchestration

The longer-term design should support many MT5 accounts while preserving centralized risk and audit controls.

Previous architecture work considered scaling toward approximately 100 accounts, with execution distributed across multiple MT5 execution nodes.

A representative scaling pattern discussed was:

- 4 MT5 execution servers × 25 accounts, or
- 2 execution nodes × 50 accounts.

This is a scaling target/context, not an instruction to implement 100-account live trading immediately.

Multi-account concerns include:

- account registry,
- per-account permissions and limits,
- allocation rules,
- synchronization,
- execution fan-out,
- failure isolation,
- broker/account-specific constraints,
- central monitoring.

### 4.9 Monitoring and audit

Every material trading decision should be traceable.

Capture:

- market inputs,
- derived features,
- detected setup,
- reasoning output,
- risk decision,
- final order parameters,
- execution response,
- account/position state,
- errors,
- timestamps,
- model/version identifiers where applicable.

---

## 5. Data, research, and model-development direction

The architecture should support a research pipeline covering:

`Historical Data -> Processing -> Feature Engineering -> Training/Research -> Backtesting -> Validation -> Model Registry -> Controlled Deployment`

Topics discussed for future development include:

- historical trading datasets,
- model training,
- deep learning / reinforcement-learning research where justified,
- ensembles,
- walk-forward testing,
- Monte Carlo robustness testing,
- model registry/versioning,
- continuous evaluation.

The objective is not to let a model simply “learn live” without controls. Research, validation, and deployment should be separated.

---

## 6. Backtesting and evaluation philosophy

Before meaningful live deployment, strategies and model-assisted decisions should pass structured evaluation.

Expected evaluation areas include:

- historical backtesting,
- out-of-sample testing,
- walk-forward analysis,
- Monte Carlo/stress testing,
- transaction-cost assumptions,
- slippage assumptions,
- regime sensitivity,
- drawdown analysis,
- risk-adjusted performance,
- failure-case analysis.

Avoid optimizing only for headline win rate or raw profit.

---

## 7. Initial implementation direction / MVP

The initial MVP direction discussed includes:

- Python-based backend,
- FastAPI or equivalent API service,
- market-data ingestion,
- XAUUSD market-structure analysis,
- liquidity-sweep detection,
- ATR/volatility analysis,
- AI reasoning layer,
- deterministic risk and position sizing,
- MT5 Expert Advisor / execution bridge skeleton,
- API authentication,
- automated tests,
- setup/developer instructions.

This describes the initial technical direction and should be implemented incrementally rather than as one monolithic task.

---

## 8. Development phasing

The project should be built sequentially.

### Phase 0 — Architecture and planning

Status: **completed for initial handoff**

Completed at this stage:

- overall system concept,
- architecture,
- component boundaries,
- development approach,
- roadmap/phasing,
- Codex implementation role,
- Claude review role,
- GitHub shared-context approach.

### Phase 1 — Foundation

Goal: establish the repository and software foundation.

Likely work includes:

- project/package structure,
- configuration management,
- typed domain models,
- logging,
- test framework,
- secrets/environment conventions,
- API skeleton,
- basic documentation.

### Phase 2 — Market-data foundation

Goal: reliable normalized inputs.

Likely work includes:

- provider interface,
- canonical OHLCV/events,
- timestamp normalization,
- symbol mapping,
- data validation,
- mock/test data,
- health/retry logic.

### Phase 3 — Deterministic market analysis

Goal: encode the core analytical building blocks.

Likely work includes:

- market structure,
- swing detection,
- ATR/volatility context,
- displacement detection,
- impulse/correction logic,
- liquidity-sweep detection,
- setup/event schemas.

### Phase 4 — Strategy and AI reasoning

Goal: create structured setup evaluation and model-assisted reasoning.

Likely work includes:

- setup candidate creation,
- context packaging,
- reasoning interface,
- explainable outputs,
- confidence/quality fields,
- strict output schemas.

### Phase 5 — Risk engine

Goal: deterministic pre-trade control.

Likely work includes:

- position sizing,
- trade-level risk,
- exposure limits,
- drawdown limits,
- validation/rejection rules,
- kill-switch behavior.

### Phase 6 — MT5 execution

Goal: controlled order execution.

Likely work includes:

- execution API/bridge,
- EA skeleton,
- authentication,
- order lifecycle,
- idempotency,
- reconciliation,
- failure handling.

### Phase 7 — Backtesting and validation

Goal: evaluate the integrated trading logic rigorously.

Likely work includes:

- historical replay,
- transaction costs/slippage,
- walk-forward testing,
- stress testing,
- performance reporting.

### Phase 8 — Paper trading / controlled forward testing

Goal: verify behavior in live market conditions without unrestricted capital risk.

Likely work includes:

- paper/demo environment,
- monitoring dashboards,
- alerting,
- audit review,
- failure drills.

### Phase 9 — Multi-account scaling

Goal: scale only after earlier layers are stable.

Likely work includes:

- account registry,
- allocation/fan-out,
- multi-node execution,
- centralized exposure controls,
- failure isolation,
- operational monitoring.

The exact task granularity inside each phase can be refined as the repository grows, but implementation should respect the dependency order.

---

## 9. Security and secrets

Never commit:

- broker passwords,
- MT5 credentials,
- API keys,
- private keys,
- database credentials,
- unrestricted webhook secrets,
- personally identifiable live account data.

Use environment variables, secret stores, and repository/platform secrets.

The existing Python `.gitignore` is a baseline; extend it when new tooling introduces local secret/config artifacts.

---

## 10. ChatGPT, Codex, Claude, and GitHub responsibilities

### ChatGPT — architect / planner

Primary responsibilities:

- system architecture,
- requirements,
- trading-system design,
- phase/task decomposition,
- architecture decisions,
- strategy specification,
- review of system-level consistency.

ChatGPT should convert accepted planning discussions into repository-native context as needed.

### Codex — implementation engineer

Primary responsibilities:

- code implementation,
- tests,
- debugging,
- refactoring,
- repository changes,
- documentation updates tied to implementation,
- preparing clean commits/PRs.

Codex should implement the approved architecture rather than silently redesign it.

If implementation reveals a genuine architecture problem, Codex should document the issue and propose a change rather than quietly changing system boundaries.

### Claude — independent reviewer

Primary responsibilities:

- code review,
- architecture-compliance review,
- bug detection,
- edge-case identification,
- security review,
- test-quality review,
- maintainability review.

Claude is intended primarily as a reviewer, not as a second full-context implementation agent.

To control token/credit consumption, Claude should normally receive:

1. relevant task context,
2. relevant architecture excerpt/file,
3. Git diff or PR,
4. test results,
5. any specific review questions.

Claude should not repeatedly consume the complete historical project context unless truly necessary.

### GitHub — shared communication/context layer

GitHub is the persistent bridge between separate ChatGPT, Codex, and Claude conversations.

The conversations themselves remain separate.

The repository transfers accepted context through:

- Markdown documentation,
- source code,
- tests,
- commits,
- branches,
- pull requests,
- review comments.

Principle:

**Conversations are for exploration; the repository is for accepted project truth.**

---

## 11. Codex–Claude collaboration workflow

Preferred workflow:

`ChatGPT architecture/task -> GitHub -> Codex implementation -> GitHub PR/diff -> Claude review -> Codex corrections -> merge`

Detailed sequence:

1. ChatGPT defines/refines architecture or an implementation task.
2. Accepted context is recorded in GitHub.
3. Codex reads the relevant repository context.
4. Codex implements the scoped task and adds tests.
5. Codex produces a clean diff/commit/PR.
6. Claude reviews the focused diff plus relevant architecture/task context.
7. Review findings return through GitHub or are provided back to Codex.
8. Codex addresses actionable findings.
9. Final architecture and implementation checks occur.
10. Changes are merged.

This reduces duplicate context consumption and lets GitHub serve as the neutral handoff surface.

---

## 12. Important architectural constraints

Codex should preserve these constraints unless explicitly changed:

- AI reasoning is not the final risk authority.
- Deterministic risk controls sit between reasoning and execution.
- Execution should consume validated, risk-approved instructions.
- MT5-specific logic should not contaminate core strategy/reasoning modules.
- Market-data provider specifics should be isolated behind adapters/interfaces where practical.
- Research/training and production execution should remain separable.
- Observability and auditability are first-class requirements.
- Multi-account scaling is a later phase, not the starting implementation target.

---

## 13. Current project status

The project is at the transition point between **completed initial architecture/planning** and **implementation**.

At the time of this handoff:

- the GitHub repository has been created,
- the Python `.gitignore` exists,
- no production trading implementation should be assumed,
- the architecture/planning context is being transferred into the repository,
- Codex has been connected to GitHub,
- Claude is intended to be used later as a focused reviewer.

---

## 14. What Codex should do first

Codex should not begin by rebuilding or redesigning the architecture.

Initial action:

1. read `AGENTS.md`,
2. read this handoff in full,
3. inspect the repository,
4. summarize its understanding of:
   - system boundaries,
   - phase sequence,
   - AI/risk separation,
   - Codex/Claude workflow,
5. identify the first implementation task within **Phase 1 — Foundation**,
6. propose the smallest coherent implementation slice,
7. wait for/obtain the implementation instruction before making broad architectural changes.

Once the initial repository context is established, future tasks should use the repository itself as the primary project context.
