# Project status and backlog

## Current milestone: Phase 1 — first spell cast

The user reports that Phase 0 and the first spell-data import checkpoint were successful on the Windows PC. Accept those local results. The next task is [one valid and rejected spell cast](PHASE_1_02_FIRST_CAST.md), before adding a full deck, UI, enemy turn, or remaining spells. The data import does not complete the full Phase 1 duel milestone.

The Windows Unreal project reached GitHub `main` in commit `56fafc5` (`Set up Project Arcana third-person prototype`). It includes ProjectArcana.uproject, Config, the saved courtyard, and Third Person content. All 527 Unreal binary asset paths in that commit are LFS pointers, representing about 1,017 MiB of unique asset content. The connected cloud checkout was synchronized with this history, preserving the user's files. It has no Unreal Editor; most asset bodies remain unmaterialized LFS pointers because the current cloud work is documentation and source inspection.

The newly created `ST_SpellDefinition` and `DT_Spells` binary assets are not in that shared commit; the user reports their local import completed. Do not recreate or overwrite those assets just because they are not yet on GitHub. One unfamiliar untracked Windows filename was previously reported; leave it unstaged and do not delete it without inspection.

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
- [x] Confirm the real Unreal project files have reached GitHub: commit `56fafc5`, including the .uproject, Config, saved map, and template assets.
- [x] Record engine, hardware, and the user-reported local result here. Exact measured performance and final rendering values were not separately supplied.

Phase 0 local gameplay setup is accepted as passed from the user's report, and the initial project upload is confirmed. No assistant-run Unreal test is implied by this record.

## Immediate next task

Create `BP_CombatController` as an Actor Blueprint in `Content/Arcana/Combat`, following [the node-by-node guide](PHASE_1_02_FIRST_CAST.md). Add a synchronous `TryCastSpell` function that reads the existing Data Table, validates the living target, definition, and resource cost, and commits damage/resource changes only after validation. Start at enemy health 100 and resources 1: Comet Pin should produce 70 and 0. Another cast at zero resources must return failure without changing either value.

Use a temporary BeginPlay self-test to check the valid cast, insufficient resources, defeated target, and missing spell row, then reset the test state. Keep Print String in the test/presentation graph, outside the rules function. Record the actual results before adding Pass, explicit round phases, and enemy action selection. The first-cast guide is prepared; its Blueprint and runtime checks remain unverified until performed in Unreal.

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

Still needed: first-cast Blueprint creation and actual test results, plus a later upload of the imported data assets and new controller. Hardware, storage, engine information, Phase 0 success, initial project upload, and first data-import success have been supplied or verified; do not ask for them again. Use 5.5.4 and do not switch engines or add paid tools.

## Verification record

- Repository inspection: initially empty checkout; no `.uproject` or Unreal assets available to open.
- Cloud source-control setup: Git LFS installed and initialized locally; initialization rerun successfully. Confirmed 18 inclusion/exclusion cases, 5 LFS attribute cases, 8 local Markdown links, and 7 preserved content directories.
- Prepared setup files were uploaded to GitHub `main` in commit `6923247`, preserving its initial `Create main` commit and placeholder file. The Windows Unreal project upload is confirmed in `56fafc5`.
- Phase 0 local editor/movement outcome: **passed, user-reported**. Individual logs and measured rendering performance are not supplied. The shared .uproject associates with UE 5.5; patch 5.5.4 is user-reported. Both default maps point to L_FirstDuel_Courtyard; the default GameMode is BP_ThirdPersonGameMode.
- Assistant-run Unreal compilation, editor startup, movement, packaging, and combat tests: **not run**. No packaged/offline launch test was reported.
- Phase 1 spell data: CSV prepared and checked in the cloud; Unreal import/reopen checkpoint **passed, user-reported**. Runtime lookup and spell execution **not yet tested**.
- GitHub source upload: .uproject, Config, and all 527 LFS pointer entries **confirmed**. Selected courtyard, character, and GameMode binary objects were fetched and SHA256-verified (3 objects, 158,578 bytes). Full binary materialization and editor execution in the cloud are not established by this targeted check.
- Official Epic documentation retrieval: blocked by the cloud proxy (403); version-sensitive menu instructions require confirmation in the installed 5.5.4 editor.

Update this record after real checks. Include what ran, its result, and any remaining error; do not mark an item complete based on intent.
