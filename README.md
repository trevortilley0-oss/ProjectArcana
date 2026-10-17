# Project Arcana

An original, offline, single-player 3D fantasy RPG for Windows, using Unreal Engine 5 and Blueprints. The first milestone is **The First Duel**: exploration, an enemy encounter, and a tactical card battle with six original spells.

**Selected engine: Unreal Engine 5.5.4**, already installed on the Windows development PC. Use that version for the initial project; do not upgrade during Phase 0.

**Current state:** Phase 0 gameplay setup passed on the Windows PC, as reported by the user. Phase 1 begins with [the first spell-data task](docs/PHASE_1_01_SPELL_DATA.md). The shared repository still has no `.uproject` or Blueprint assets; synchronizing the Windows project is a separate outstanding task. No Unreal Editor or Windows gameplay check has run in the cloud environment.

1. Continue with [Phase 1: the first spell definition](docs/PHASE_1_01_SPELL_DATA.md).
2. Use [project status](docs/STATUS.md) to track actual results and the outstanding source upload.
3. Keep [the Windows setup guide](docs/PHASE_0.md) as a reference for recreating Phase 0.

## Project notes

- [Original project requirements](docs/PROJECT_BRIEF.md)
- [Current milestone, decisions, and backlog](docs/STATUS.md)
- [Planned system boundaries and first six spells](docs/ARCHITECTURE.md)

`Content/Arcana/` reserves folders for original game assets. Empty `.gitkeep` files preserve folders in Git; they are not Unreal assets. Create Blueprints, maps, Data Assets, and effects in Unreal Editor.

The GitHub repository is `trevortilley0-oss/ProjectArcana`. For a new checkout, clone it to get the prepared files and follow the Windows setup guide. The user's existing Windows project should be preserved and uploaded, not recreated. Keep the existing `main` history when committing generated project files.
