# Goal-to-Workspace Builder

## What is this?

This is a tool that helps you figure out whether your work is a good fit for a system
called MWP (Model Workspace Protocol), and if it is, builds that system for you.

**MWP in one paragraph:** It's a way to organize repeating work that involves an AI agent.
Instead of writing software, you organize folders and text files. The folder structure tells
the AI what to do at each step. You review the output between steps and keep, edit, or redirect
as needed. The result is a pipeline you own and can adjust with a text editor — no code required.

This workspace is built on the methodology described in
[Interpretable Context Methodology: Folder Structure as Agent Architecture](https://drive.google.com/file/d/1cMubR3UrpsXlXIg-2br3Qp8Z2hTc9LKp/view)
by Jake Van Clief and David McDermott.

---

## Is this for me?

MWP works best for work that is:
- **Repeating** — you do this same shape of work regularly, with different input each time
- **Sequential** — step 2 depends on step 1
- **Worth reviewing** — there are natural points where you'd want to check the output before continuing

If that sounds like something you do — writing reports, preparing deliverables, analyzing
inputs, producing content — run this pipeline and find out.

---

## What do I do?

**Step 1.** Open `setup/goal-intake.md` and answer the questions. Be as brief or detailed
as you like. You can leave gaps — Stage 1 will make assumptions and flag them.

**Step 2.** Run Stage 1 — either with the skill or by prompting your agent directly.

> With the skill: `/run-stage 1`
>
> Without the skill: "Please run Stage 1. Read CLAUDE.md first, then CONTEXT.md, then follow
> the contract in stages/01_assess_design/CONTEXT.md."

Stage 1 will assess whether MWP is a good fit for your goal. If it is, it proposes
a workflow design — the stages, the reference files, and the skills for your new workspace.

**Step 3.** Open `stages/01_assess_design/output/workflow-proposal.md` and review the
proposed design. Edit it freely — add stages, remove stages, rename them, adjust anything.
This is the one review gate that shapes everything that follows.

Stage 1 will produce one of four outputs:

| Output file | What it means |
|---|---|
| `workflow-proposal.md` | MWP fits — review the proposed design, then run Stage 2 |
| `split-recommendation.md` | Goal should become 2+ MWPs — read for how to divide it and which to build first |
| `scope-report.md` | Goal is too large for MWP — read for how to reduce scope or which framework to use instead |
| `no-go-report.md` | MWP isn't the right tool — read for alternatives |

Only `workflow-proposal.md` continues to Stage 2. Edit it freely before proceeding.

**Step 4.** Run Stage 2 to generate the workspace files.

> `/run-stage 2`

Stage 2 produces a `workspace-manifest.md` — every file in your new workspace in one place.

**Step 5.** Run Stage 3 to validate the manifest before anything is written to disk.

> `/run-stage 3`

Stage 3 checks the manifest for broken references, wrong layer labels, missing skills,
incomplete configure.md, and other issues — and fixes them in place. When it's clean,
run `/write-workspace` to create the actual folder and files on disk.
When prompted for a location, name the folder after your workflow
(e.g., `article-pipeline`, `weekly-report`). It will be created as a sibling of this folder.

**Step 6.** Open the new workspace folder and follow the instructions in
`setup/configure.md` to fill in your preferences before running the pipeline.

---

## What do I get?

**If your work is a good fit:** a complete, ready-to-use MWP workspace tailored to your goal.
Every file is written. You open a new folder, fill in your preferences once, and run it.

**If it's not a good fit:** a clear explanation of why MWP doesn't fit this particular goal,
and suggestions for what might work better.

---

## Skills

These slash commands are available in Claude Code. Type them directly in the chat.

| Skill | When to use it |
|---|---|
| `/new-run` | Starting a new goal — clears all stage outputs and prompts you to update the intake |
| `/run-stage [1\|2\|3]` | Run a specific stage (or omit the number to run the next pending one) |
| `/pipeline-status` | Check which stages are done and what comes next |
| `/reset-stage [1\|2\|3]` | Clear one stage's output to re-run it without clearing everything else |
| `/write-workspace` | Write the validated manifest to actual files on disk |

**Typical flow for a new goal:**
```
/new-run        → update setup/goal-intake.md
/run-stage 1    → review stages/01_assess_design/output/workflow-proposal.md
/run-stage 2    → (review manifest if you want)
/run-stage 3    → validate and fix
/write-workspace
```

**If something needs rework:**
```
/reset-stage 1  → edit goal-intake.md or the previous output, then re-run
/run-stage 1
```

---

## Files in this workspace

| File | What it is |
|---|---|
| `README.md` | This file — start here |
| `setup/goal-intake.md` | Fill this in before running Stage 1 |
| `stages/01_assess_design/` | Assesses fit and proposes a workflow design for review |
| `stages/02_build/` | Generates all workspace files from the approved design |
| `stages/03_validation/` | Validates the manifest and fixes issues before writing to disk |
| `_config/mwp-conventions.md` | The rules that govern all MWP workspaces |
| `_config/skills-catalog.md` | Skills available to recommend and generate for new workspaces |
| `.claude/skills/` | Slash commands for running and managing this workspace |
