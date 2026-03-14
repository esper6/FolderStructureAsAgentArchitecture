# Stage 3: Validation

Validate the workspace manifest against MWP conventions and fix any issues
before the workspace is written to disk.

## Inputs
- L4 (working):   ../02_build/output/workspace-manifest.md — the manifest to validate
- L4 (working):   ../01_assess_design/output/workflow-proposal.md — the approved design to check against
- L3 (reference): ../../_config/mwp-conventions.md — the rules to validate against

## Process

First check that `stages/02_build/output/workspace-manifest.md` exists. If it doesn't,
check whether Stage 1 produced a non-proposal output (scope-report, split-recommendation,
or no-go-report) and tell the user Stage 3 only runs after a successful Stage 2 build.

Otherwise, parse `workspace-manifest.md` into its file blocks (each block is `## workspace/[filepath]`
through the next `---`). Build a complete list of every filepath and its content.
Then run each check below. For each: record Pass or Fail, and if Fail, the specific
location and what's wrong.

After all checks, fix every failure directly in the manifest — edit the relevant file
block in place. Then re-run the failed checks to confirm the fix. Do not report failures
without fixing them.

---

### Check 1 — Cross-reference integrity

Every relative path listed in any stage contract's `## Inputs` section must resolve to
a file that exists in the manifest.

For each `stages/0N_*/CONTEXT.md` block in the manifest:
- Extract every path from the Inputs section
- Verify that path appears as a `## workspace/[filepath]` block in the manifest
- Flag any path that does not resolve

---

### Check 2 — No circular dependencies

Trace the reference graph across all stage contracts.
If Stage A lists Stage B's output as an input, Stage B must not list Stage A's output.
Confirm the dependency graph is a straight chain with no cycles.

---

### Check 3 — Stage handoff chain

Verify the pipeline is unbroken:
- Each stage (except the last) has at least one output file
- That output file is referenced as an input by the next stage's CONTEXT.md
- List the full chain and flag any gap

---

### Check 4 — Layer labels

Every `## Inputs` section in every stage contract must use `L3` for reference material
from `_config/` and `L4` for working artifacts from a previous stage's `output/`.
Flag any line that uses `L2` or any other label.

---

### Check 5 — CLAUDE.md navigation

The `workspace/CLAUDE.md` block must contain a `## Navigation` section with a line
pointing to `CONTEXT.md`. Flag if absent.

---

### Check 6 — CONTEXT.md routing

The `workspace/CONTEXT.md` block must reference each stage folder that exists in
the manifest. Flag any stage folder present in the manifest but missing from CONTEXT.md.

---

### Check 7 — Skill coverage

From `workflow-proposal.md`, extract the list of slam dunk skills.
Verify that each has a corresponding `## workspace/.claude/skills/[name]/SKILL.md`
block in the manifest. Flag any missing.

---

### Check 8 — configure.md completeness

The `workspace/setup/configure.md` block must include a checklist entry for every
`_config/` file that appears in the manifest. Flag any `_config/` file not listed.

---

### Check 9 — No placeholder text

Scan every file block in the manifest for:
- The literal strings `[insert here]`, `[TODO]`, `TODO`, `[placeholder]`
- Any bracket-wrapped phrase longer than 3 words (e.g. `[describe your workflow here]`)

Flag the filepath and the offending line for each match.

---

### Check 10 — CONTEXT.md purity

Stage contract blocks (`stages/0N_*/CONTEXT.md`) must contain only:
`# Stage N: Name`, `## Inputs`, `## Process`, `## Outputs`.
Flag any block that contains definitions, extended rules, examples, or guidelines
that belong in `_config/` instead.

---

### Check 11 — Naming conventions

- All stage folder names in the manifest are lowercase-with-hyphens and zero-padded
  (e.g. `01_research`, `02_draft`)
- All file and folder names are lowercase (no camelCase, no spaces)
- Flag any violation

---

### Check 12 — Skill frontmatter

Every `SKILL.md` block in the manifest must contain a YAML frontmatter section
(delimited by `---`) with at minimum a `name` and `description` field.
Flag any SKILL.md missing frontmatter or missing either required field.

---

## Output format

Write the validation report as:

```
# Validation Report

## Summary
- Checks run: 12
- Passed: N
- Failed: N (fixed: N)

## Results

### Check 1 — Cross-reference integrity: PASS
### Check 2 — No circular dependencies: PASS
### Check 4 — Layer labels: FAIL → FIXED
  - workspace/stages/01_research/CONTEXT.md, line 8: `L2 (reference)` → changed to `L3 (reference)`
...

## Manifest status
Ready for /write-workspace.
```

If all checks pass (or all failures were fixed), close with:
"Manifest is valid. Run `/write-workspace` to create the workspace folder and files on disk."

If any failure could not be fixed (e.g. a referenced file is genuinely missing from the
design), flag it clearly and tell the user what to correct in `workflow-proposal.md`
before re-running Stage 2.

## Outputs
- validation-report.md → output/
