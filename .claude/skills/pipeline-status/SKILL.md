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
