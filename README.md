# Project Arcana

An original, offline, single-player 3D fantasy RPG for Windows, using Unreal Engine 5 and Blueprints. The first milestone is **The First Duel**: exploration, an enemy encounter, and a tactical card battle with six original spells.

**Selected engine: Unreal Engine 5.5.4**, already installed on the Windows development PC. Use that version for the initial project; do not upgrade during Phase 0.

**Current state:** Phase 0 repository preparation. This repository contains setup instructions and an initial content folder structure. It does not yet contain an Unreal project, Blueprint assets, a playable character, or a build. No Unreal Editor or Windows gameplay check has run in the cloud environment.

1. Follow [the numbered Windows setup guide](docs/PHASE_0.md).
2. Complete its movement test before starting combat implementation.
3. Record the engine version and results in [project status](docs/STATUS.md).

## Project notes

- [Original project requirements](docs/PROJECT_BRIEF.md)
- [Current milestone, decisions, and backlog](docs/STATUS.md)
- [Planned system boundaries and first six spells](docs/ARCHITECTURE.md)

`Content/Arcana/` reserves folders for original game assets. Empty `.gitkeep` files preserve folders in Git; they are not Unreal assets. Create Blueprints, maps, Data Assets, and effects in Unreal Editor.

The GitHub repository is `trevortilley0-oss/ProjectArcana`. Clone it to get these prepared files, then follow the Windows setup guide to add the real Unreal project. Keep the existing `main` history when committing generated project files.
