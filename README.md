# Project Arcana

An original, offline, single-player 3D fantasy RPG for Windows, using Unreal Engine 5 and Blueprints. The first milestone is **The First Duel**: exploration, an enemy encounter, and a tactical card battle with six original spells.

**Selected engine: Unreal Engine 5.5.4**, already installed on the Windows development PC. Use that version for the initial project; do not upgrade during Phase 0.

**Current state:** Phase 0 gameplay setup and the first spell-data import passed on the Windows PC, as reported by the user. The Third Person project is now on GitHub. Next is [Phase 1: the first spell cast](docs/PHASE_1_02_FIRST_CAST.md): read Comet Pin from the Data Table, spend resources, apply damage, and reject invalid casts. The first-cast Blueprint and its tests are not yet implemented or verified. No Unreal Editor or Windows gameplay check has run in the cloud environment.

1. Continue with [Phase 1: the first spell cast](docs/PHASE_1_02_FIRST_CAST.md).
2. Use [project status](docs/STATUS.md) to track actual results. Keep the locally created Structure and Data Table when updating from GitHub; their binary assets are not in the shared version yet.
3. Keep [the Windows setup guide](docs/PHASE_0.md) as a reference for recreating Phase 0.

## Project notes

- [Original project requirements](docs/PROJECT_BRIEF.md)
- [Current milestone, decisions, and backlog](docs/STATUS.md)
- [Planned system boundaries and first six spells](docs/ARCHITECTURE.md)

`Content/Arcana/` reserves folders for original game assets. Empty `.gitkeep` files preserve folders in Git; they are not Unreal assets. Create Blueprints, maps, Data Assets, and effects in Unreal Editor.

The GitHub repository is `trevortilley0-oss/ProjectArcana`. A new clone now includes the Third Person project: download its Git LFS assets and open `ProjectArcana.uproject` in UE 5.5.4. Do not generate a replacement template. Preserve the user's existing Windows project and locally imported spell assets. Keep the existing `main` history when committing changes.
