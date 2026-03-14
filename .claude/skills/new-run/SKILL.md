---
name: new-run
description: Clear all stage outputs to start a fresh run for a new goal
user-invocable: true
---

Reset this workspace for a new goal description.

1. List all output files currently in stages/01_assess_design/output/,
   stages/02_build/output/, and stages/03_validation/output/
2. If all output folders are already empty, tell the user the workspace is already
   clean and ready. Stop.
3. Show the user what will be deleted and ask: "Clear all stage outputs and start
   fresh? This cannot be undone."
4. If confirmed:
   - Delete all files in each output/ folder except .gitkeep
5. Tell the user:
   "All outputs cleared. Update setup/goal-intake.md with your new goal,
   then run /run-stage 1 to begin."
