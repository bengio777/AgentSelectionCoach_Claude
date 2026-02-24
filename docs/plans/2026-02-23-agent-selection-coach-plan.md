# Agent Selection Coach — Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Build a Claude Code skill that coaches users through selecting the right agent architecture pattern via guided interview.

**Architecture:** Single skill directory with SKILL.md (coaching logic + embedded frameworks) and references/ directory (decision matrix, pattern catalog). Mirrors the prompt-coach skill structure.

**Tech Stack:** Claude Code skill (Markdown), no code dependencies.

---

### Task 1: Scaffold the Skill Directory

**Files:**
- Create: `~/.claude/skills/agent-selection-coach/SKILL.md` (empty placeholder)
- Create: `~/.claude/skills/agent-selection-coach/references/pattern-catalog.md`
- Create: `~/.claude/skills/agent-selection-coach/references/decision-matrix.md`

**Step 1: Create the directory structure**

```bash
mkdir -p ~/.claude/skills/agent-selection-coach/references
touch ~/.claude/skills/agent-selection-coach/SKILL.md
touch ~/.claude/skills/agent-selection-coach/references/pattern-catalog.md
touch ~/.claude/skills/agent-selection-coach/references/decision-matrix.md
```

**Step 2: Verify structure**

Run: `find ~/.claude/skills/agent-selection-coach -type f`
Expected: Three files listed.

**Step 3: Commit**

```bash
cd ~/AgentSelectionCoach_Claude
git add -A && git commit -m "chore: scaffold skill directory structure"
```

---

### Task 2: Write the Decision Matrix Reference

**Files:**
- Write: `~/.claude/skills/agent-selection-coach/references/decision-matrix.md`

**Step 1: Write decision-matrix.md**

Content includes:
- **Complexity Ladder** — Levels 0-3 with definitions, "choose when" criteria, and example tasks for each level.
- **Signal-to-Pattern Map** — The 8 pattern-signal mappings as a table.
- **Complexity Escalation Checklist** — The 4 diagnostic questions.
- **Anti-Patterns** — All 8 anti-patterns with one-line descriptions.

Source material: Sections 2, 6, 11, and 14 from `Agent-Builder-Master-Reference.md`. Sections 1, 5, and 11 from `Anthropic_building-effective-agents-reference.md`.

Distill, don't copy. The coach needs to reference these frameworks conversationally, not read them verbatim.

**Step 2: Review for completeness**

Verify all 4 complexity levels, 8 pattern signals, 4 escalation questions, and 8 anti-patterns are present.

**Step 3: Commit**

```bash
cd ~/AgentSelectionCoach_Claude
git add -A && git commit -m "feat: add decision matrix reference"
```

---

### Task 3: Write the Pattern Catalog Reference

**Files:**
- Write: `~/.claude/skills/agent-selection-coach/references/pattern-catalog.md`

**Step 1: Write pattern-catalog.md**

For each of the 8 patterns, write a structured entry:
- **Pattern name**
- **How it works** (2-3 sentences)
- **Choose this when** (bullet list of signals)
- **Real-world examples** (2-3)
- **Key trade-offs** (cost, latency, complexity, error risk)
- **Common pitfall** (the anti-pattern most associated with this choice)

Patterns to cover:
1. Single LLM Call
2. Augmented LLM
3. Prompt Chaining
4. Routing
5. Parallelization (Sectioning + Voting)
6. Orchestrator-Workers
7. Evaluator-Optimizer
8. Autonomous Agent

Source material: Sections 3-5 from `Agent-Builder-Master-Reference.md`. Sections 2-4 from `Anthropic_building-effective-agents-reference.md`.

**Step 2: Review for completeness**

All 8 patterns have all 6 fields filled in. Trade-offs are specific, not generic.

**Step 3: Commit**

```bash
cd ~/AgentSelectionCoach_Claude
git add -A && git commit -m "feat: add pattern catalog reference"
```

---

### Task 4: Write the Main SKILL.md

**Files:**
- Write: `~/.claude/skills/agent-selection-coach/SKILL.md`

**Step 1: Write the YAML frontmatter**

```yaml
---
name: agent-selection-coach
description: >
  Interactive agent architecture coach that guides users through selecting the
  right pattern — from single LLM call to autonomous agent — via structured
  conversation. Use this skill when someone wants to build an agent, design an
  AI workflow, select an architecture pattern, or says things like "should I
  build an agent?", "what pattern should I use?", "help me choose an
  architecture", or describes a task they want to automate with AI. Also
  triggers when users are over-engineering (reaching for agents when simpler
  patterns suffice). This skill coaches through the decision — it does not
  build the agent.
---
```

**Step 2: Write the skill body**

Structure (following prompt-coach as the structural pattern):

1. **Role definition** — "You are an expert agent architecture coach..."
2. **Core principle** — The complexity ladder principle. Most valuable intervention is catching over-engineering early.
3. **Coaching Stages** — Full Stage 1-5 flow from the design doc. Each stage includes:
   - Purpose
   - Questions to ask (one at a time, prefer multiple choice)
   - What to listen for in answers
   - When to push back
   - How to transition to next stage
4. **Output format** — Decision path + structured recommendation template (from design doc)
5. **Prompt Coach handoff** — Reference to Prompt Coach skill at the end of the output
6. **Behavior rules** — All 8 rules from the design doc
7. **Reference file pointers** — `references/decision-matrix.md` and `references/pattern-catalog.md` with instructions on when to read each

The skill should read `references/decision-matrix.md` during Stage 2 (complexity check) and Stage 3 (signal matching). It should read `references/pattern-catalog.md` during Stage 4 (trade-off discussion) and Stage 5 (recommendation delivery).

**Step 3: Verify the skill file is valid**

Check: YAML frontmatter parses correctly. All 5 stages are present. Output template is complete. Handoff reference is included.

**Step 4: Commit**

```bash
cd ~/AgentSelectionCoach_Claude
git add -A && git commit -m "feat: add main SKILL.md with coaching flow"
```

---

### Task 5: Copy Skill to Repo for Version Control

**Files:**
- Create: `~/AgentSelectionCoach_Claude/skill/` (mirror of `~/.claude/skills/agent-selection-coach/`)

**Step 1: Copy skill files to repo**

```bash
cp -r ~/.claude/skills/agent-selection-coach ~/AgentSelectionCoach_Claude/skill
```

**Step 2: Commit**

```bash
cd ~/AgentSelectionCoach_Claude
git add -A && git commit -m "chore: add skill files to repo for version control"
```

---

### Task 6: Smoke Test — Invoke the Skill

**Step 1: Invoke the skill in Claude Code**

Start a new Claude Code session and say: "I want to build something that processes customer support tickets automatically."

**Step 2: Verify the coaching flow**

Check:
- Coach asks one question at a time
- Stage 2 complexity check fires (this task could be a workflow, not an agent)
- Coach pushes back if user reaches for agent complexity
- Signal matching narrows to a pattern
- Final output includes decision path + structured recommendation
- Prompt Coach handoff reference appears at the end

**Step 3: Fix any issues found**

If the flow breaks, adjust SKILL.md and recommit.

---

### Task 7: Push to GitHub

**Step 1: Push**

```bash
cd ~/AgentSelectionCoach_Claude
git push -u origin main
```

**Step 2: Verify**

Run: `gh repo view bengio777/AgentSelectionCoach_Claude --web`

---
