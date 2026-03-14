# Goal-to-Workspace Builder

## What this is
An MWP workspace that takes a goal description and produces a ready-to-run MWP workspace
tailored to that goal. One agent reads the right files at the right stage. No framework code.

## Layers
- **L0** — This file: workspace identity ("where am I?")
- **L1** — `CONTEXT.md`: task routing ("where do I go?")
- **L2** — `stages/0N_*/CONTEXT.md`: stage contracts ("what do I do?")
- **L3** — `_config/`: stable reference material ("what rules apply?")
- **L4** — `stages/0N_*/output/`: working artifacts ("what am I working with?")

## Navigation
Read `CONTEXT.md` next to find the right workflow and starting stage.
