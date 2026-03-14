---
name: write-workspace
description: Write the generated workspace files to disk from the Stage 3 manifest
user-invocable: true
---

Write the workspace files from the Stage 3 manifest to disk.

1. Check `stages/03_workspace_build/output/` for output files.
   - If `no-go-summary.md` exists, read it and tell the user MWP was not recommended
     for their goal. Show the summary. Stop.
   - If `workspace-manifest.md` does not exist, tell the user Stage 3 hasn't been run yet
     and suggest running `/run-stage 3` first. Stop.

2. Read `stages/03_workspace_build/output/workspace-manifest.md` in full.

3. Ask the user: "Where should I create the workspace? Enter a folder name or path.
   I'll create it as a sibling of this workspace folder."
   Suggest a default name based on the workflow described in the manifest.

4. For each file block in the manifest — blocks that start with `## workspace/[filepath]`
   and end with `---` — do the following:
   - Extract the filepath by stripping the leading `workspace/` prefix
   - Resolve it relative to the user's chosen destination folder
   - Create any necessary parent directories
   - Write the file content exactly as it appears between the header and the `---` separator

5. After all files are written, print:
   - The total number of files created
   - The full path to the new workspace folder
   - "Next step: open [workspace]/setup/configure.md and follow the setup instructions
     before running the pipeline."
