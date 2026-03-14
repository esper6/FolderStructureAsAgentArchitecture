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
