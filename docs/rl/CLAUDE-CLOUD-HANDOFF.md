# Hermes — Offline policy learning (research only)

**Status:** PLANNED / EXPERIMENTAL. No training or integration approved.

## Background and boundaries
This fork already includes experience-based skills, memory, and trajectory-related functionality. Those are **not** reinforcement learning. This proposal explores bounded policy selection using synthetic trajectories, offline evaluation, and a deterministic baseline, **without modifying LLM weights** or executing real tools.

This is **separate** from the DeepSeek Recipe / Anthropic / Responses phase-2 work. Phase-2 patches are BLOCKED after an independent adversarial audit, chiefly due to F01 (synthetic secret recovered from unredacted sidecar data). Do not consume, merge, or circumvent those candidate patches.

## M0 — Audit and design
Inspect actual repository interfaces for tool selection, trajectory collection, skills, policy validation, and telemetry. Produce:
- `docs/rl/ARCHITECTURE.md` — data contracts, allowed actions, observation/reward semantics, reproducibility and fallback.
- `docs/rl/THREAT_MODEL.md` — secrets, PII, reward manipulation, tool permission boundaries, dataset poisoning, rollout hazards.
- ADR comparing deterministic rules, contextual bandits, and offline RL. Prefer simplest defensible option.

## M1 — Offline evaluator
Create a **synthetic**, versioned dataset and deterministic baseline with tests. Measure per-scenario:
- correct tool selection / success
- unsafe or invalid decisions
- estimated latency/cost (only when evidence supports estimates)
- evaluation leakage and reward hacking indicators

No live model, GPU, network, expensive API, real tool execution, or production traces. Do not invent performance improvements.

## M2 — Opt-in research prototype (conditional)
Only after M1 is reproducible: a contextual bandit or bounded offline policy for selecting among **already authorized** tools or execution strategies. Default off. The model/policy **must never** grant permissions, change sandbox constraints, rewrite prompts/safety policy, edit credentials, retrain weights, or deploy itself. Unknown/unsafe input -> fixed baseline/fail closed.

## M3 — Decision
Run unit/differential/adversarial tests and document `docs/rl/EVALUATION.md` and `docs/rl/DECISION.md`. Compare to frozen baseline and list FAIL/BLOCKED/NOT TESTED. Keep work as draft PR; **no merge, deployment or change to main**. Independence from blocked phase-2 patches required.

## Claude Code Cloud task
Inspect the repo first. Implement M0 + M1 in this branch, then M2 only if gated evidence warrants it. Prefer stdlib and existing dependencies; do not add global tooling or secrets. Create a draft PR against `main` with exact commands and evidence. If access is unavailable, state BLOCKED rather than guessing. Do not approve your own changes for production.
