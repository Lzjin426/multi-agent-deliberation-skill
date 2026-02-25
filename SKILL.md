---
name: multi-agent-deliberation
description: Run multi-agent deliberation when a task has arguable alternatives, testable evaluation criteria, and a falsifiable evidence chain. Use this skill to improve final answer quality and robustness via proposer/critic/checker/judge workflow with structured verdict, confidence, evidence map, residual risks, and verification steps.
---

# Multi-Agent Deliberation

## Overview

Use this skill to run a structured multi-agent deliberation workflow for quality-first answers.

Prioritize this skill only when debate adds value. Do not use it for simple factual queries or low-stakes prompts where a single agent is sufficient.

## Workflow Decision Tree

1. Validate activation gate.
2. Define the decision target and testable criteria.
3. Instantiate debate roles.
4. Run bounded rounds with evidence constraints.
5. Judge with fixed rubric.
6. Emit final answer with uncertainty and risk controls.

## Step 1: Validate Activation Gate

Require all three conditions before using multi-agent debate:

1. Confirm arguable space.
2. Confirm testable criteria.
3. Confirm a falsifiable evidence chain.

If any condition is missing, switch to single-agent response and explain why debate was not activated.

Load `references/gate_and_controls.md` for the gate checklist and fallback logic.

## Step 2: Define Evaluation Contract

Set these items before spawning roles:

1. Define `QUESTION` and explicit `CONSTRAINTS`.
2. Define acceptance metrics.
3. Define source quality policy.
4. Define budget limits (rounds, token, latency).

Use this default rubric unless the user specifies otherwise:

- Accuracy: 40%
- Completeness: 25%
- Traceability: 20%
- Robustness after rebuttal: 15%

## Step 3: Instantiate Debate Roles

Create exactly five roles by default:

1. `Proposer-A`: deliver candidate answer A with evidence map.
2. `Proposer-B`: deliver independent candidate answer B with evidence map.
3. `Critic`: attack both candidates and expose hidden assumptions.
4. `Evidence-Checker`: verify claim-source consistency and mark unsupported claims.
5. `Judge`: score and decide with fixed rubric.

Keep A/B isolated during first draft to reduce early convergence.

Load `references/debate_playbook.md` for reusable prompt blocks.

## Step 4: Run Bounded Deliberation

Run rounds with strict limits:

1. Round 0: independent proposals (A/B).
2. Round 1: critic cross-examination.
3. Round 2: targeted rebuttal and evidence updates.
4. Optional Round 3: allow only if new evidence appears or core disagreement remains unresolved.

Terminate early when marginal gain is low:

- Stop when improvement is <1% for two consecutive scoring checkpoints.
- Stop when no new evidence appears.

## Step 5: Judge and Decide

Force structured decision output:

1. Provide `SCORE_A` and `SCORE_B`.
2. Select winner or merged final with explicit merge rationale.
3. Emit final answer with confidence and residual risks.
4. Emit what evidence would change the decision.

Never force consensus if evidence is weak.

## Step 6: Output Contract

Always output this structure:

```text
[FINAL_ANSWER]
...

[CONFIDENCE_0_TO_100]
...

[KEY_EVIDENCE_MAP]
- claim -> source
...

[OPEN_RISKS]
...

[WHAT_WOULD_CHANGE_THE_ANSWER]
...

[NEXT_VERIFICATION_STEPS]
...
```

## Operating Controls

1. Cap rounds at 2 by default; max 3.
2. Cap per-role response length to avoid verbosity bias.
3. Require source IDs for critical claims.
4. Reject unsupported claims at judge step.
5. Escalate to human review for high-stakes unresolved conflicts.

Load `references/gate_and_controls.md` for failure modes and mitigation rules.

## Resource Loading Guide

Load only what is needed:

1. Load `references/research_findings_2026-02-25.md` when deciding whether debate is justified and for parameter defaults.
2. Load `references/debate_playbook.md` when you need role prompts or output templates.
3. Load `references/gate_and_controls.md` when you need activation checks, stop rules, and risk governance.