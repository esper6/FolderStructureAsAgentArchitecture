# Skills Catalog

Reference material for recommending and generating Claude Code skills in generated workspaces.
Skills live at `.claude/skills/<skill-name>/SKILL.md` within the workspace folder.

---

## Universal Skills

Include these in every generated workspace regardless of domain.
The SKILL.md content below is ready to use as-is.

---

### `run-stage`

**When to include:** Always. This is how the user runs the pipeline.

**SKILL.md content:**
```
---
name: run-stage
description: Run a specific stage of this MWP pipeline
argument-hint: "[stage number or name]"
user-invocable: true
---

Run a stage of this MWP pipeline.

1. Read CLAUDE.md to orient yourself — understand the workspace structure and what stages exist
2. Read CONTEXT.md to find the right stage folder
3. If a stage number or name was provided in $ARGUMENTS, go to that stage's folder.
   If no argument was given, check each stages/0N_* folder for output/ files.
   Run the first stage whose output/ folder is empty.
4. Read that stage's CONTEXT.md in full
5. Load all files listed in the Inputs section
6. Execute the Process section exactly as described
7. Write the output file(s) to the stage's output/ folder
8. Tell the user what was produced and invite them to review it before running the next stage
```

---

### `pipeline-status`

**When to include:** Always. Gives the user a clear view of where they are.

**SKILL.md content:**
```
---
name: pipeline-status
description: Show which stages of this pipeline have been completed and what comes next
user-invocable: true
---

Report the current status of this MWP pipeline.

1. Read CLAUDE.md to get the list of stages
2. For each stage folder in stages/, check whether output/ contains any non-empty files
3. Print a status table:
   - Stage number and name
   - Status: Done / Pending / Ready to run
   - Output file name if done, or "—" if pending
4. Identify the next stage to run and tell the user how to run it
```

---

### `reset-stage`

**When to include:** Always. Lets users re-run a stage after editing inputs or instructions.

**SKILL.md content:**
```
---
name: reset-stage
description: Clear a stage's output so it can be re-run
argument-hint: "[stage number or name]"
user-invocable: true
---

Clear the output of a stage so it can be run again with updated inputs or instructions.

1. Identify the stage from $ARGUMENTS (number or name)
2. List the files currently in that stage's output/ folder
3. Confirm with the user before deleting: "This will delete [files]. Proceed?"
4. If confirmed, delete the output files
5. Tell the user the stage is ready to re-run with /run-stage $ARGUMENTS
```

---

## Domain-Specific Skills

Stage 2 selects from these based on the goal. For each recommended skill, Stage 3 generates
a full SKILL.md tailored to the specific workflow.

The entries below describe what each skill does and when to recommend it.
Stage 3 writes the actual SKILL.md content based on these descriptions plus the workflow design.

---

### Content / Writing

**`brainstorm`**
- What it does: Generates a set of topic or angle ideas based on a brief or theme, formatted
  as a numbered list the user can pick from or edit
- When to recommend: Workflows where the user needs to choose a topic or angle before starting
- Slam dunk for: Article writing, newsletter, social content, presentation pipelines

**`polish`**
- What it does: Reviews a draft for tone, clarity, pacing, and adherence to voice guidelines,
  then applies targeted edits
- When to recommend: Any writing pipeline where a final edit stage would benefit from
  an explicit instruction set rather than a general "make it better" prompt
- Slam dunk for: Article, report, or email pipelines with a defined voice

**`headline-variants`**
- What it does: Generates 5–10 alternative headlines or titles for a draft, with brief
  notes on the angle each one takes
- When to recommend: Pipelines producing content where the headline is a distinct decision
- Slam dunk for: Article, email subject line, social post pipelines

---

### Research / Analysis

**`summarize-source`**
- What it does: Takes a URL, document, or pasted text and produces a structured summary
  (key points, evidence, gaps) formatted as the pipeline's expected input format
- When to recommend: Pipelines where the user needs to bring in external sources before
  the main workflow starts
- Slam dunk for: Research, competitive analysis, literature review pipelines

**`gap-check`**
- What it does: Reviews a research or analysis output and identifies what's missing,
  what's weakly supported, and what questions remain unanswered
- When to recommend: Pipelines with a research or analysis stage where completeness matters
- Slam dunk for: Report, brief, due diligence pipelines

---

### Data / Reporting

**`data-validate`**
- What it does: Reviews structured input data (CSV, JSON, or pasted table) for missing
  fields, obvious errors, and format consistency before the pipeline runs
- When to recommend: Pipelines where bad input data would silently produce bad output
- Slam dunk for: Any pipeline that processes structured data as its primary input

**`format-check`**
- What it does: Compares a finished output against the expected format spec and flags
  any deviations before the user publishes or sends it
- When to recommend: Pipelines with strict output format requirements (templates, reports
  that go into a system, regulated documents)
- Slam dunk for: Compliance, operations, and publishing pipelines

---

### Workflow / Process

**`stage-diff`**
- What it does: Shows what changed between two stages — compares a stage's input to its
  output and summarizes the transformation in plain language
- When to recommend: Pipelines where the user wants to understand what the agent actually
  did at a stage, not just read the output
- Slam dunk for: Any pipeline where trust-building matters (new users, high-stakes output)

**`retry-with-note`**
- What it does: Re-runs the current stage with an additional user note appended to the
  stage's instructions, without permanently modifying the CONTEXT.md
- When to recommend: Pipelines where users frequently want to redirect a stage mid-run
  without editing the source contract
- Slam dunk for: Creative pipelines with variable output expectations

---

## Domain Skill SKILL.md Template

When Stage 3 generates a domain skill, use this template as the scaffold.
Fill in every bracketed section from the workflow design and goal document.
Do not leave any bracket unfilled.

```
---
name: [skill-name — lowercase, hyphenated]
description: [one sentence: what this skill does and when Claude should use it automatically]
argument-hint: "[what the user passes as an argument, if anything — omit line if none]"
user-invocable: true
---

[One sentence stating what this skill does in this specific workspace.]

1. Read [specific file(s) relevant to this skill — use actual filenames from the workflow]
   to understand [what context is needed].
2. [Main action — be specific: what does the agent produce, decide, or transform?]
3. [Any validation, formatting, or secondary action.]
4. Write the result to [output location or tell the user inline].
5. Tell the user what was produced and suggest the next step.
```

Keep skill instructions short and specific. A skill should do one thing.
If the catalog description says the skill "generates X based on Y", the process steps
should name the actual X and Y from this workspace.

---

## Skill Recommendation Format for Stage 2 Output

When Stage 2 identifies skills, output a Skills section in workflow-design.md:

```
## Skills

### Slam dunk (include in every run of this workspace)
| Skill | Source | Reason |
|---|---|---|
| run-stage | Universal | Primary way to execute pipeline stages |
| pipeline-status | Universal | Lets user check progress |
| reset-stage | Universal | Lets user re-run a stage after edits |
| [domain skill] | Domain | [one sentence: why it's an obvious fit] |

### Conditional (recommend to user; they decide)
| Skill | Condition | What it adds |
|---|---|---|
| [domain skill] | [when this would be useful] | [what it enables] |
```
