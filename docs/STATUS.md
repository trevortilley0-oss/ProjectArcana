# Project status and backlog

## Current milestone: Phase 0 — Setup

The connected checkout began with no project files or commits. The current work establishes source-control configuration and beginner setup documentation. A runnable Unreal project is not present, and no game feature has passed an editor test.

The development direction is practical: Unreal Engine 5 with Blueprints can support the requested offline Windows prototype. The reported CPU, RAM, discrete GPU, and available SSD space are well suited to this scope. Unreal Engine **5.5.4** is installed and selected for Phase 0. Actual editor behavior remains to be checked. This cloud environment has no Unreal Editor or access to that Windows machine.

## Windows development PC — reported by the user

| Item | Reported specification |
| --- | --- |
| Operating system | Windows 11 Pro 25H2 |
| CPU | AMD Ryzen 7 7800X3D, 8 cores, 4.20 GHz |
| RAM | 32 GB (31.1 GB usable) |
| Dedicated GPU | NVIDIA GeForce RTX 5080, 16 GB VRAM |
| Integrated GPU | AMD Radeon Graphics, reported 476 MB; use the RTX 5080 for Unreal |
| Free SSD space | 300 GB reported available |
| Unreal Engine | 5.5.4 installed; selected engine version |

These specifications are user-reported, not inspected remotely. Start with the Scalable template, Medium viewport settings, and no hardware ray tracing to keep the first check simple; measure performance before increasing quality.

## Phase 0 completion checklist

- [x] Review the supplied brief and inspect the initial repository state.
- [x] Record the confirmed direction, future system boundaries, and backlog.
- [x] Record Windows version, CPU, RAM, and dedicated GPU/VRAM from the user's report.
- [x] Confirm free SSD space and installed Unreal version from the user's report: 300 GB and 5.5.4.
- [x] Select the existing Unreal Engine 5.5.4 installation; no reinstall or upgrade needed.
- [ ] Create a **Games → Third Person → Blueprint** project named `ProjectArcana` using the settings in [the setup guide](PHASE_0.md).
- [ ] Open the project successfully and create the initial content folders.
- [ ] Press **Play**, demonstrate movement, camera control, and jumping, then stop and reopen the project successfully.
- [ ] Check that source-control exclusions work and the project files are available in the intended GitHub repository.
- [ ] Record the engine version, hardware, chosen rendering settings, and actual editor verification results here.

Phase 0 remains open until the character movement test passes on the Windows PC. Creating documentation is not equivalent to creating or testing a `.uproject` or `.uasset`.

## Immediate next task

Follow [the Windows setup guide](PHASE_0.md) using the installed **5.5.4** engine to create the smallest Third Person Blueprint project and verify movement in the editor. If the editor fails, capture the exact error and fix that prerequisite before adding combat. Report the actual results so this checklist can be updated with evidence.

## Ordered backlog

| Phase | Smallest useful outcome | Required evidence before moving on |
| --- | --- | --- |
| 0 — Setup | A Third Person Blueprint character moves in the editor. | Project opens, movement/camera/jump work, source control ignores generated data. |
| 1 — Combat prototype | One player fights one AI using a hand of up to seven cards, resources, deck/discard, six original spells, and a basic interface. | Required combat cases pass; a complete duel can be won or lost in the editor. |
| 2 — Exploration loop | A small courtyard encounter connects exploration, battle, a short quest, rewards/experience, and local save/load. | Launch → explore → encounter → resolve → return/restart works, with saves verified. |
| 3 — Expanded gameplay | Astral, Verdant, and Runic support at least twelve spells, deck customization, and basic progression. | Each added mechanic is playable and checked against combat rules. |
| 4 — Polished vertical slice | Consistent original environment, characters, effects, UI, sound, animation, and pacing. | The complete slice runs acceptably on the selected hardware and quality settings. |
| 5 — Full RPG expansion | Additional story, zones, equipment, dungeons, bosses, and other justified systems. | Expand from the verified slice in small playable increments. |
| 6 — Multiplayer experiment | A small cooperative battle prototype after assessing the existing architecture. | A separate networking design and tested authority model; no multiplayer server now. |

The eventual **First Duel** milestone combines the combat and courtyard loop. It requires editor execution and tested outcomes, not just written code or design documents.

## Decisions and open inputs

Confirmed: Windows PC; $0; offline single player first; Unreal Engine 5; Blueprints first; stylized original fantasy; card-based turn combat; local saves; no current server. See [ARCHITECTURE.md](ARCHITECTURE.md) for boundaries and provisional spell examples. Do not ask to reconfirm these decisions during routine setup.

Still needed from the Windows machine: actual editor launch and movement test results. All requested hardware, storage, and engine information has been supplied; do not ask for it again. Use 5.5.4, retain the reported hardware record, and do not switch engines or add paid tools.

## Verification record

- Repository inspection: initially empty checkout; no `.uproject` or Unreal assets available to open.
- Cloud source-control setup: Git LFS installed and initialized locally; initialization rerun successfully. Confirmed 18 inclusion/exclusion cases, 5 LFS attribute cases, 8 local Markdown links, and 7 preserved content directories.
- Prepared setup files are intended for the existing GitHub `main` branch, preserving its initial `Create main` commit and placeholder file. Windows source-control commands still require execution on the Windows checkout.
- Unreal compilation, editor startup, movement, rendering performance, packaging, and combat tests: **not run**.
- GitHub source upload is separate from editor validation. Windows source-control behavior and actual Unreal binary asset uploads remain **unverified**.
- Official Epic documentation retrieval: blocked by the cloud proxy (403); version-sensitive menu instructions require confirmation in the installed 5.5.4 editor.

Update this record after real checks. Include what ran, its result, and any remaining error; do not mark an item complete based on intent.
