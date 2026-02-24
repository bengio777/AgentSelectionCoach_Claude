# Agent Selection Coach — Design Document

> **Date:** 2026-02-23
> **Status:** Approved
> **Repo:** AgentSelectionCoach_Claude

---

## Overview

An interactive Claude Code skill that coaches users through selecting the right agent architecture pattern for their task. Uses a guided interview (one question at a time) to walk users from "do you even need an agent?" through to a specific pattern recommendation with reasoning.

## Core Outcome

User walks away with an architecture recommendation — the right pattern (single LLM call, workflow type, or autonomous agent) — with reasoning, trade-offs, and next steps. Not a full build spec.

## Approach

**Linear Funnel (Approach A)** — Every user walks the same structured path top to bottom. The "do you even need an agent?" challenge is the highest-value intervention and must not be skippable.

## Target Audience

All levels — HOAI course students, general Claude users, and experienced builders. Coach adapts explanation depth based on user's demonstrated familiarity.

## Delivery

Claude Code skill (`~/.claude/skills/`). Self-contained with embedded knowledge frameworks. No external file dependencies at runtime.

## Handoff

The Agent Selection Coach stops at architecture recommendation. For prompt engineering within the selected pattern, the output references the Prompt Coach skill as the next step.

---

## Coaching Flow — Five Stages

### Stage 1: Task Discovery
- "Describe the task or workflow you want to build."
- "What does success look like? How will you know it's working?"
- Purpose: Capture raw intent before any architecture discussion.

### Stage 2: Complexity Check
- Coach evaluates the task against the Complexity Ladder (Levels 0-3).
- Diagnostic questions: "Can this be done in a single prompt?" / "Are the steps predictable or open-ended?" / "Does it need tools or memory?"
- If the answer is Level 0 or 1, coach **actively pushes back** — recommends the simpler path with reasoning, citing the Complexity Escalation Checklist.
- User can override, but the coach makes the case.

### Stage 3: Signal Matching
- Maps task characteristics to pattern signals from the Decision Matrix.
- Asks targeted questions based on ambiguity: "Are the subtasks independent or sequential?" / "Can you define clear evaluation criteria?" / "Do different inputs need different handling?"
- Narrows to 1-2 candidate patterns.

### Stage 4: Trade-off Discussion
- Surfaces key trade-offs for recommended pattern(s): cost, latency, complexity, error risk.
- References anti-patterns if relevant.
- If task straddles two patterns, presents both with the distinction.

### Stage 5: Recommendation Delivery
- Outputs the structured recommendation (see Output Format below).

---

## Output Format

The recommendation delivers two parts — decision path visualization, then structured summary.

```
## Decision Path

Task: [user's task description, paraphrased]

START -> Do you need an agent?
  |-- Single prompt sufficient? -> No
  |-- Needs tools/memory (one step)? -> No
  |-- Fixed sequence of steps? -> No
  |-- Different inputs need different handling? -> No
  |-- Independent subtasks run concurrently? -> Yes
  +-- MATCH -> Parallelization (Sectioning)

## Recommendation

**Pattern:** Parallelization (Sectioning)
**Complexity Level:** Level 2 -- Workflow (Predefined Orchestration)

### Why This Fits
- [2-3 sentences connecting user's specific task signals to the pattern]

### Key Trade-offs
| Consideration | Impact |
|---|---|
| Latency | [specific to their case] |
| Cost | [specific to their case] |
| Complexity | [specific to their case] |
| Error risk | [specific to their case] |

### What to Watch For
- [1-2 relevant anti-patterns or pitfalls]

### Next Steps
1. [Concrete first action]
2. [Second action]
3. [Third action]

> Ready to craft prompts for this pattern? Use the **Prompt Coach** skill
> to engineer effective prompts for each component.
```

The decision path shows every branch evaluated — checked and rejected branches visible so the user sees the full reasoning. The trade-off table is personalized to the user's task, not generic.

---

## Embedded Knowledge

The skill embeds these frameworks directly (distilled, not full reference docs):

1. **Complexity Ladder** (Levels 0-3) — Definitions + "choose when" criteria for each level. Gating logic for Stage 2.

2. **Decision Matrix** — 8 pattern-signal mappings:
   - Single step, clear I/O -> Single LLM call
   - Fixed sequence, dependent steps -> Prompt Chaining
   - Distinct input categories -> Routing
   - Independent concurrent subtasks -> Parallelization (Sectioning)
   - Multiple perspectives needed -> Parallelization (Voting)
   - Subtasks depend on input analysis -> Orchestrator-Workers
   - Clear quality criteria, iterative improvement -> Evaluator-Optimizer
   - Fully open-ended, environment feedback -> Autonomous Agent

3. **Pattern Descriptions** — One-paragraph summary of each pattern: purpose, how it works, when to choose it.

4. **Anti-Patterns** — 8 anti-patterns the coach references contextually:
   - Premature complexity
   - Framework lock-in
   - Poor tool design
   - Missing ground truth
   - No sandbox
   - Ignoring cost/latency
   - Monolithic prompts
   - Overloading with tools

5. **Complexity Escalation Checklist** — 4 questions used when pushing back:
   - Fully optimized prompt/retrieval at current level?
   - Added complexity measurably improves outcomes?
   - Can test and debug reliably?
   - Latency/cost tradeoff acceptable?

**Out of scope:** Tool design guidance, system prompt templates, implementation details, multi-agent orchestration specifics.

---

## Behavior Rules

1. **One question per message.** Never stack multiple questions. Prefer multiple-choice when possible.
2. **Push back on complexity.** If Stage 2 lands at Level 0 or 1, make the case for simplicity before proceeding. User can override.
3. **Cite the framework.** Reference specific principles when recommending or pushing back. Teaches, not just advises.
4. **Personalize trade-offs.** Trade-off table uses the user's task specifics, not generic descriptions.
5. **Acknowledge uncertainty.** If the task straddles two patterns, recommend both with the distinction.
6. **No implementation advice.** Stops at architecture recommendation + next steps. Does not design tools, write prompts, or produce build plans.
7. **Adapt tone to experience.** Brief explanations for experienced users, more context for newcomers.
8. **Handoff to Prompt Coach.** Final output includes a reference to the Prompt Coach skill for the next phase.
