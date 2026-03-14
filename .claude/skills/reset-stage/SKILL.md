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
4. If confirmed, delete the output files (leave .gitkeep in place)
5. Tell the user the stage is ready to re-run with /run-stage $ARGUMENTS
