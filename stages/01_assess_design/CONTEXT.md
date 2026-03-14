# Stage 1: Assess & Design

Evaluate whether the goal warrants an MWP, and if it does, propose a complete workflow
design for the user to review and edit before any files are generated.

## Inputs
- L4 (working):   ../../setup/goal-intake.md — the user's goal description
- L3 (reference): ../../_config/mwp-conventions.md — fit criteria, layer architecture, stage contract format
- L3 (reference): ../../_config/skills-catalog.md — available skills to recommend

## Process

Read `goal-intake.md` in full.

---

### Step 1 — Assess fit

Evaluate in the order defined in `mwp-conventions.md`. Stop at the first match and
produce the corresponding output. Stage 2 only runs if the outcome is Recommended or Borderline.

---

**Check A — Too large?**

Does the goal show any "too large" signals from `mwp-conventions.md`?
List each signal present and one sentence explaining why it applies.

If yes, write `scope-report.md → output/` containing:
- Which signals were triggered and why, in plain language
- **Path 1 — Reduce scope**: what to cut or simplify so MWP can handle the core.
  Write a narrowed goal statement the user can paste back into `setup/goal-intake.md`
  and re-run Stage 1.
- **Path 2 — Use a framework**: recommend the right tool based on what the goal needs:
  - Concurrent agents → AutoGen or CrewAI
  - Complex orchestration with memory and tool use → LangChain
  - Role-based agent teams → CrewAI
  - Simple automation with branching → n8n, Zapier, or a Python script
  Include one sentence on why that tool fits this goal specifically.

Stop here. Stage 2 does not need to run.

---

**Check B — Should be multiple MWPs?**

Does the goal show two or more "split" signals from `mwp-conventions.md`?
List each signal present with one sentence of reasoning.

If yes, write `split-recommendation.md → output/` containing:

**Proposed MWPs** — one section per proposed workspace:
- Name (descriptive, not technical)
- What it does (one sentence)
- What triggers a run
- What it takes as input
- What it produces
- Who runs it

**Interface between them** — what file or artifact connects the MWPs.
Describe it concretely: filename, format, where it lives, which MWP writes it,
which reads it. This is the handoff point the user maintains between runs.

**Build order** — which MWP to build first and why (usually the one whose output
feeds the others).

**Next step** — "Run this workspace builder again for each piece, starting with
[first MWP name]. Fill in `setup/goal-intake.md` with just that piece's goal."

Stop here. Stage 2 does not need to run.

---

**Check C — Single MWP fit**

Only reach this check if A and B were both clear.

Score each criterion: **Yes / Partial / No**, with one sentence of reasoning.
1. **Repeatable** — same shape of work, different input each time?
2. **Sequential** — step 2 depends on step 1?
3. **Review-worthy** — natural points where a human would check and possibly redirect?

- **Recommended** — all Yes or Partial → proceed to Step 2
- **Borderline** — one No → proceed to Step 2, include a Fit Warning
- **Not recommended** — two or more No → write `no-go-report.md → output/` containing:
  - Why MWP doesn't fit, in plain language
  - 1–2 alternatives better suited to the goal
  - What would need to change for MWP to become appropriate
  - Stop here. Stage 2 does not need to run.

---

### Step 2 — Propose the workflow

Produce a structured proposal the user can review and edit before Stage 2 runs.

**Goal statement** — one sentence:
"This pipeline takes [input] and produces [output] for [audience], running [cadence]."

**Technical level** — note the user's comfort level from the intake. Stage 2 uses this
to calibrate the language of every generated file.

**Fit warning** (borderline only) — plain language: which criterion was weak, what to
watch for, and one concrete action if it becomes a problem.

**Proposed stages** — for each stage:

| # | Folder name | One-line job | Reads | Writes | Review likelihood | Why its own stage |
|---|---|---|---|---|---|---|

Follow the stage boundary criteria from `mwp-conventions.md`. Aim for 2–4 stages.
For each stage, "Review likelihood" is: Almost always / Sometimes / Rarely — and why.
"Why its own stage" is what would go wrong if it were merged with the adjacent stage.

**Proposed _config/ files** — table:

| Filename | What it controls |
|---|---|

List only files genuinely needed. One row per distinct concern (voice, structure, conventions).
Don't add files the user won't actually fill in.

**Proposed skills** — use the Skill Recommendation Format from `skills-catalog.md`:
- Slam dunk: the three universal skills + any domain skills that are obvious fits
- Conditional: domain skills that depend on which direction the user takes the workspace

If a useful skill isn't in the catalog, propose it: name, what it does, which category.

**Open questions** — anything unclear from the intake, each with a default assumption
clearly marked as an assumption. Stage 2 will use the assumptions if the user doesn't
correct them.

---

Close the output with this exact line:

> Review the proposed stages, _config files, and skills above. Edit any section freely —
> add stages, remove stages, rename them, adjust the config list, change skill recommendations.
> When you're satisfied with the design, run Stage 2 to generate the workspace files.

## Outputs
- workflow-proposal.md → output/      (Check C: Recommended or Borderline)
- no-go-report.md → output/           (Check C: Not recommended)
- split-recommendation.md → output/   (Check B: Should be multiple MWPs)
- scope-report.md → output/           (Check A: Too large for MWP)
