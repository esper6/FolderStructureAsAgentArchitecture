# Stage 2: Build

Generate the complete workspace files from the user-reviewed design proposal —
or summarise the no-go if Stage 1 determined MWP is not the right fit.

## Inputs
- L4 (working):   ../01_assess_design/output/workflow-proposal.md — the reviewed design
- L3 (reference): ../../_config/mwp-conventions.md — file formats and conventions to follow
- L3 (reference): ../../_config/skills-catalog.md — universal skill content + domain skill template

## Process

Check `stages/01_assess_design/output/` for which file Stage 1 produced, then follow
the matching path below.

---

### If Stage 1 produced `scope-report.md`, `split-recommendation.md`, or `no-go-report.md`

Stage 2 does not need to run. Tell the user:
"Stage 1 determined this goal isn't ready to build as a single MWP.
Read `stages/01_assess_design/output/[filename]` for guidance on next steps."

Stop here.

---

### If Stage 1 produced `workflow-proposal.md`

Read it in full.

---

### If it contains a workflow proposal

Generate a workspace manifest: one file containing the complete content of every file
in the new workspace, ready to be written to disk.

Write real, usable content for every file. No placeholders. No "[insert here]".
Use the goal statement, technical level, stages, _config list, and skills from the proposal.

**Calibrate all language to the technical level** noted in the proposal:
- Non-technical: plain language, no jargon, step-by-step instructions, friendly prompts
- Technical: concise, can use layer names and MWP terminology

**If the proposal contains a Fit Warning**, include it verbatim in the generated
`README.md` under a "Known limitations" heading, and in `setup/configure.md` under
a "Before you start" note above the checklist.

---

Generate these files in this order:

**`workspace/README.md`**
- What this workspace does (one paragraph, plain language)
- **Before you run anything** — prominent section pointing to `setup/configure.md`
- How to run the pipeline each time (fill in the intake, then run stages in order)
- What they get at the end
- File table: every file and what it's for
- **Skills section** — required, must include all three of:

  1. A table of every slam dunk skill using `/skill-name` syntax:
     | Skill | What it does |
     |---|---|
     | `/run-stage [N]` | ... |
     | `/pipeline-status` | ... |
     | `/reset-stage [N]` | ... |
     | `/[domain-skill]` | ... |

  2. A **Typical flow** block showing the exact slash commands in order:
     ```
     /run-stage 1    → review [output file], edit if needed
     /run-stage 2    → ...
     /write-workspace
     ```

  3. A **If something needs rework** block:
     ```
     /reset-stage [N]   → edit [intake or previous output], then re-run
     /run-stage [N]
     ```

  For conditional skills: a separate table with the condition and the instruction
  "To add: create `.claude/skills/[name]/SKILL.md` and ask your agent to generate it
  using the domain skill template, or copy it from the MWP skills catalog."

**`workspace/CLAUDE.md`** — L0
- What this is (goal statement from the proposal)
- Layers table (L0 through L4)
- Navigation: "Read CONTEXT.md next"

**`workspace/CONTEXT.md`** — L1
- Workflows section with stage table (number, folder, one-line job)
- Shared resources section (_config files and their paths)

**`workspace/_config/[name].md`** — one per file in the proposed _config/ list
- Real template with section headers and guidance inside each section
- Guidance tells the user what to put there during setup
- Calibrate detail to the user's technical level

**`workspace/setup/configure.md`** — one-time setup checklist
- What this workspace does (one sentence)
- Checklist: every _config/ file, what it controls, what "done" looks like
- "When all items are checked, you are ready to run Stage 1."
- "You only need to do this setup once."

**`workspace/setup/[intake-file].md`** — entry point for each run
- Goal-specific prompts, pre-filled with any known context
- Examples calibrated to the domain
- Complexity calibrated to the user's technical level

**`workspace/stages/0N_name/CONTEXT.md`** — one per stage in the proposal
Follow the Stage Contract Format from `mwp-conventions.md` exactly:
- Stage number and name as H1
- Inputs section with L3/L4 labels, relative paths, and purpose notes
- Process section: specific instructions written for the agent — what to read, what to
  decide, what the output should contain, how to handle edge cases
- Outputs section

**`workspace/.claude/skills/[name]/SKILL.md`** — one per slam dunk skill
- Universal skills (run-stage, pipeline-status, reset-stage): copy verbatim from `skills-catalog.md`
- Domain skills from catalog: use the Domain Skill SKILL.md Template from `skills-catalog.md`;
  fill every bracket with specifics from this workflow (actual stage names, file names, domain context)
- Proposed new skills: generate from the proposal's description using the same template

---

**Output format** — sequence of file blocks:

```
## workspace/README.md

[full file content]

---

## workspace/CLAUDE.md

[full file content]

---

## workspace/CONTEXT.md

[full file content]

---

## workspace/setup/configure.md

[full file content]

---

## workspace/setup/[intake-file].md

[full file content]

---

## workspace/_config/[name].md

[full file content]

---

## workspace/stages/01_name/CONTEXT.md

[full file content]

---

## workspace/.claude/skills/run-stage/SKILL.md

[full file content]

---
```

Each block starts with `## [filepath]` and ends with `---`.

After writing the manifest, tell the user:
"Your workspace manifest is ready at `stages/02_build/output/workspace-manifest.md`.
Run Stage 3 to validate and fix the manifest before writing files to disk."

## Outputs
- workspace-manifest.md → output/  (if MWP is recommended)
- no-go-summary.md → output/       (if not recommended)
