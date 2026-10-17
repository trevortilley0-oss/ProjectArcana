# Project status and backlog

## Current milestone: Phase 1 — first combat-data task

The user reports that Phase 0 was successful on the Windows PC. Accept that report as the local setup and movement result. Begin Phase 1 with [one imported spell definition](PHASE_1_01_SPELL_DATA.md), then a tiny one-spell combat test before adding the full deck, UI, or remaining spells.

The connected checkout and GitHub `main` still contain only the prepared setup files at the latest inspection. The Windows `.uproject`, Config, and binary assets have not appeared there. Local gameplay success and source upload are separate: finish synchronizing the project without asking the user to repeat the completed movement exercise.

The development direction is practical: Unreal Engine 5 with Blueprints can support the requested offline Windows prototype. The reported CPU, RAM, discrete GPU, and available SSD space are well suited to this scope. Unreal Engine **5.5.4** is installed and selected. The Windows editor result is user-reported; this cloud environment has no Unreal Editor or access to that Windows machine.

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
- [x] Create and open the Third Person Blueprint project — covered by the user's Phase 0 success report.
- [x] Complete the setup guide's movement/reopen checks — user-reported Phase 0 success, not independently observed.
- [ ] Confirm the real Unreal project files have reached the intended GitHub repository. Cloud Git rules passed checks; the Windows upload is not yet visible.
- [x] Record engine, hardware, and the user-reported local result here. Exact measured performance and final rendering values were not separately supplied.

Phase 0 local gameplay setup is accepted as passed from the user's report. Source synchronization remains open. No assistant-run Unreal test is implied by this record.

## Immediate next task

Create `ST_SpellDefinition`, import `SourceData/Spells/DT_Spells.csv` as `DT_Spells`, and verify the Comet Pin row using [the exact editor steps](PHASE_1_01_SPELL_DATA.md). This is a data checkpoint, not a working spell or battle.

In parallel, inspect `git status --short --branch` on the Windows checkout to diagnose the missing project upload. Do not overwrite or reset local project files. Once the data import is verified, the next gameplay task is a Blueprint combat controller that spends 1 spark and applies 30 damage, rejecting an unaffordable second cast without changing state.

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

Still needed: Windows Git status to diagnose the missing source upload, then actual Phase 1 import and gameplay results. Hardware, storage, engine information, and the Phase 0 success report have been supplied; do not ask for them again. Use 5.5.4 and do not switch engines or add paid tools.

## Verification record

- Repository inspection: initially empty checkout; no `.uproject` or Unreal assets available to open.
- Cloud source-control setup: Git LFS installed and initialized locally; initialization rerun successfully. Confirmed 18 inclusion/exclusion cases, 5 LFS attribute cases, 8 local Markdown links, and 7 preserved content directories.
- Prepared setup files were uploaded to GitHub `main` in commit `6923247`, preserving its initial `Create main` commit and placeholder file. The Windows Unreal project upload is not yet visible.
- Phase 0 local editor/movement outcome: **passed, user-reported**. Individual logs, measured rendering performance, and binary project files are not available in the connected checkout.
- Assistant-run Unreal compilation, editor startup, movement, packaging, and combat tests: **not run**. No packaged/offline launch test was reported.
- Phase 1 spell data: CSV prepared and checked in the cloud; Unreal import, runtime lookup, and spell execution **not yet tested**.
- GitHub source upload is separate from editor validation. Windows source-control behavior and actual Unreal binary asset uploads remain **unverified**.
- Official Epic documentation retrieval: blocked by the cloud proxy (403); version-sensitive menu instructions require confirmation in the installed 5.5.4 editor.

Update this record after real checks. Include what ran, its result, and any remaining error; do not mark an item complete based on intent.
