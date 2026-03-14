# MWP Conventions

Reference material for building MWP workspaces. Stable across all runs of this pipeline.

> Source: [Interpretable Context Methodology: Folder Structure as Agent Architecture](https://drive.google.com/file/d/1cMubR3UrpsXlXIg-2br3Qp8Z2hTc9LKp/view) — Van Clief & McDermott

---

## MWP Fit Criteria

Evaluate in this order. Stop at the first match.

---

### 1. Too large for MWP

Flag as too large if the goal shows any of these signals:

| Signal | Why it exceeds MWP |
|---|---|
| Requires concurrent agents | MWP is sequential by design; parallel work needs a framework |
| Requires automated mid-pipeline branching | "If AI output is X, do Y" without a human deciding needs code |
| Requires error recovery or retry logic | MWP has no retry mechanism; failures require manual re-run |
| Requires real-time or event-driven triggers | MWP is on-demand and human-initiated |
| Requires external system orchestration | Deployments, CI/CD, database writes need scripting or automation |
| Would need 7+ stages to cover the full scope | Complexity signal; almost always means the scope should be split or reduced |

If too large: produce a `scope-report.md` explaining which signals were triggered,
then offer two paths:
- **Reduce scope** — what to cut or simplify so MWP can handle the core workflow
- **Use a framework** — recommend LangChain (complex orchestration), AutoGen (multi-agent
  collaboration), or CrewAI (role-based agent teams) depending on what the goal needs

---

### 2. Should be multiple MWPs

Flag as a split if the goal shows two or more of these signals:

| Signal | Why it needs splitting |
|---|---|
| Two sub-workflows run at different frequencies | One factory ≠ two schedules |
| Output of one sub-workflow feeds multiple downstream workflows independently | Shared artifact = shared interface, not shared pipeline |
| Different people trigger or own different parts | Different operators = different workspaces |
| One part is fully useful without running the other | Independence = separate MWP |
| Keeping it together would require 5+ stages | Stage count is often a split signal in disguise |

If split: produce a `split-recommendation.md` proposing how to divide the goal,
what the interface between the MWPs is, and which to build first.

---

### 3. Single MWP fit criteria

Only evaluate these if the goal passed checks 1 and 2 above.

| Criterion | Question to ask | Fails when |
|---|---|---|
| **Repeatable** | Same shape of work, different input each time? | One-off task, or no stable structure across runs |
| **Sequential** | Step 2 depends on step 1, in a fixed order? | Parallel agents, automated branching, real-time responses |
| **Review-worthy** | Natural points where a human checks and possibly redirects? | Fully mechanical, or every step needs so much judgment a pipeline doesn't help |

- **Recommended** — all three Yes or Partial
- **Borderline** — one criterion is No
- **Not recommended** — two or more criteria are No

If not recommended, suggest an alternative:
- **One-off / simple task** → single prompt or chat session
- **Human-led repeating work** → checklist or SOP
- **Mechanical / branching / concurrent** → scripting or automation tools
- **Already-solved problem** → existing tool (Zapier, Notion, etc.)

---

## 5-Layer Architecture

| Layer | Location | Question it answers | Changes when |
|---|---|---|---|
| L0 | Root `CLAUDE.md` | "Where am I?" | Workspace structure changes |
| L1 | Root `CONTEXT.md` | "Where do I go?" | A new workflow or routing path is added |
| L2 | `stages/0N_*/CONTEXT.md` | "What do I do?" | A stage's instructions are refined |
| L3 | `_config/` | "What rules apply?" | Voice, conventions, or templates are updated |
| L4 | `stages/0N_*/output/` | "What am I working with?" | Every run |

Each layer has a distinct rate of change and a distinct cognitive function. Do not merge
layers to appear simpler — the separation is load-bearing. L1 earns its own file the moment
a workspace has more than one workflow, because routing ("where do I go?") is a different
question from identity ("where am I?").

---

## Folder Structure

```
workspace/
    CLAUDE.md              ← L0: identity ("where am I?")
    CONTEXT.md             ← L1: routing ("where do I go?")
    _config/               ← L3: stable reference (voice, templates, conventions)
    setup/                 ← entry point: user fills this in before running Stage 1
    stages/
        01_name/
            CONTEXT.md     ← L2: stage contract ("what do I do?")
            output/        ← L4: artifacts from this stage
        02_name/
            CONTEXT.md     ← L2
            output/        ← L4
```

Folder numbering encodes execution order. Each stage reads the previous stage's `output/`
as its primary working input.

---

## Stage Contract Format

Every `stages/0N_*/CONTEXT.md` must have these three sections:

```markdown
# Stage N: [Name]

## Inputs
- L3 (reference): [relative path] — [what to use it for]
- L4 (working):   [relative path] — [what to use it for]

## Process
[One to three paragraphs describing what the agent does. Be specific about
what to read, what decisions to make, and what the output should contain.]

## Outputs
- [filename.md] → output/
```

The Inputs table must use explicit relative paths and correct layer labels:
- L3 for stable reference material from `_config/`
- L4 for working artifacts from a previous stage's `output/`
The agent should not guess what to load.

---

## Key Principles

1. **One stage, one job** — a stage that researches does not also draft. A stage that drafts
   does not also format.

2. **Plain text interface** — all handoffs are markdown or JSON. No binary formats.

3. **Every output is an edit surface** — the human opens the output file, edits if needed,
   and the next stage picks up whatever is there.

4. **Scoped context** — each stage loads only what it needs. Do not load all reference
   material into every stage.

5. **Stable vs. per-run** — `_config/` files change at most when the workspace is reconfigured.
   `output/` files change every run.

6. **Configure the factory, not the product** — `_config/` is the factory. `output/` is what
   the factory produces each run.

---

## What makes a good stage boundary

A stage boundary is worth adding when:
- A human would naturally want to review and possibly edit before continuing
- The transformation is genuinely distinct (research ≠ drafting ≠ formatting)
- An error at this point would make the next stage's work wrong in a hard-to-fix way

A stage boundary is NOT needed when:
- The work is mechanical and unlikely to go wrong
- The human would almost never edit between these two steps
- It creates a handoff file that no one will ever read

Aim for 2–4 stages. More than 5 usually means stages should be merged.

---

## Root CLAUDE.md structure (L0)

```markdown
# [Workspace Name]

## What this is
[One paragraph: what the pipeline produces, from what input, for whom.]

## Layers
- L0 — This file: workspace identity
- L1 — CONTEXT.md: task routing
- L2 — stages/0N_*/CONTEXT.md: stage contracts
- L3 — _config/: stable reference material
- L4 — stages/0N_*/output/: working artifacts

## Navigation
[Where to find key files. Point to CONTEXT.md as the next file to read.]
```

## Root CONTEXT.md structure (L1)

```markdown
# Workspace Routing

## Workflows

### [Workflow name]
[One sentence describing what this workflow produces.]

**Entry point:** [what the user fills in or provides]

| Stage | Folder | Job |
|---|---|---|
| 1 | stages/01_name/ | [one-line job] |
| 2 | stages/02_name/ | [one-line job] |

---

## Shared resources
- [description]: [path]
```
