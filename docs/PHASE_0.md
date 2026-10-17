# Phase 0: run the first third-person character

Goal: open a real Unreal project on the Windows development PC and move the template character. Complete this check before implementing combat. The template arena is a temporary test area; the fantasy courtyard and duel are later work.

The assistant can edit the connected repository but cannot operate your Windows PC. The cloud machine has Git and Git LFS but no Unreal Editor. A folder structure or project descriptor alone would not demonstrate a working game.

## 1. Check the PC before downloading

Your reported Windows 11 Pro 25H2, Ryzen 7 7800X3D, 32 GB RAM, RTX 5080 with 16 GB VRAM, and 300 GB free SSD space are well suited to this prototype. **Unreal Engine 5.5.4 is already installed and selected.** These details are recorded in `STATUS.md`; you do not need to collect them again. Proceed to Step 2 using that existing installation.

In **File Explorer → This PC**, record free space on the SSD where Unreal will be installed. If the hardware changes later, update the record using **Settings → System → About** for CPU/RAM and **Task Manager → Performance → GPU** for the graphics card.

For future reference, installed versions appear under **Epic Games Launcher → Unreal Engine → Library**. Use **5.5.4** for this project. Do not upgrade the installation just to follow a newer tutorial. The version in `docs/STATUS.md` comes from your report.

UE5 development is demanding. Epic's published guidance has recommended approximately 32 GB RAM, 8 GB graphics memory, a quad-core processor, and a DirectX 11/12 GPU. These are planning references, not a compatibility verdict for your PC. Check the official requirements for the selected engine version. The launcher reports required installation space; leave additional space for the project, asset cache, and updates. Do not begin the download if it would fill the drive.

Required now: Windows PC, a suitable free stable UE5 installation, Git with Git LFS, and adequate disk space. Optional later: Blender for original art. Figma, external assets, C++, Visual Studio, paid plugins, and online services are not needed for the first Blueprint movement test.

## 2. Install or select Unreal Engine

**For your current PC: skip installation and select Launch on the existing UE 5.5.4 tile in Epic Games Launcher → Unreal Engine → Library.** Do not download another engine version. The installation steps below are only for recreating the environment if that installation is absent.

1. Get Epic Games Launcher from the [official Unreal download page](https://www.unrealengine.com/en-US/download), install it, and sign in to your Epic account.
2. Open **Unreal Engine → Library**. If a stable UE5 version is already installed, use it and note the exact version.
3. Otherwise, under **Engine Versions**, select **+**, open the version dropdown, and choose a production UE5 release without a **Preview** designation. Do not choose a release number based on this document; verify what is available and supported on your PC.
4. Select **Install** and an SSD location with sufficient space. Keep the editor, **Templates and Feature Packs**, and Windows support; optional debug symbols and additional target platforms are unnecessary now. Let the launcher finish its prerequisite installation. If your existing installation lacks Third Person, open its launcher tile's dropdown → **Options**, enable **Templates and Feature Packs**, and apply the change.
5. Launch that engine version. The project browser should open. Stop here if it fails, and share the error text; your reported hardware is already recorded.

Because your PC also has integrated Radeon graphics, confirm Unreal uses the **RTX 5080**. In **Windows Settings → System → Display → Graphics**, add the installed engine's `Engine\Binaries\Win64\UnrealEditor.exe` as a desktop app if it is absent, open its **Options**, and choose **High performance** with the RTX 5080 listed. The full path depends on where Epic installed your selected version. Restart the editor after changing this preference.

Menu labels can vary between releases. Use the installed version's documentation if a named control is absent. There is no need to install an Unreal source build in the cloud machine.

## 3. Put the prepared repository on Windows

Install [Git for Windows](https://git-scm.com/download/win), including Git LFS when offered. Open PowerShell and run:

```powershell
New-Item -ItemType Directory -Force C:\Dev | Out-Null
Set-Location C:\Dev
git clone https://github.com/trevortilley0-oss/ProjectArcana.git
Set-Location C:\Dev\ProjectArcana
git lfs version
git lfs install --local
git status
```

If you already have a local checkout, use it instead of cloning again. Inspect `git status` and preserve local changes before updating it with `git pull --ff-only origin main`. A fresh clone should contain this guide and the prepared content folders.

If `git lfs version` fails, install Git LFS from [its official site](https://git-lfs.com/), reopen PowerShell, and rerun `git lfs version` and `git lfs install --local`. When the repository contains committed Unreal assets, also run `git lfs pull` to download the actual binaries. The initial scaffold has no binary assets to download. Never open Unreal binary assets that remain text LFS pointers.

The prepared files come directly from GitHub when you clone or update `main`; the Phase 0 ZIP is optional. Do not replace the Windows checkout's `.git` directory with Git metadata from another machine.

Verify `README.md`, `.gitattributes`, and `docs\PHASE_0.md` now exist in the Windows checkout. If GitHub requires authentication, complete Git Credential Manager's browser sign-in. Do not paste tokens into commands or chat. Successful cloning proves read access; pushing additionally requires write access to the repository.

## 4. Create the real Blueprint project

Create the template in a separate location so the project wizard does not conflict with the repository's existing files.

1. In Unreal's project browser, select **Games → Third Person**.
2. Select **Blueprint** and target **Desktop** (sometimes labeled **Desktop / Console**).
3. Select **Scalable** quality if offered. If there is a **Variant** dropdown, select **None** for the basic movement template.
4. Disable **Raytracing** and **Starter Content** if those options appear. Keep the template's own character and movement assets.
5. Set project name to **ProjectArcana** and project location to **C:\UE-Template-Staging**. The resulting project should be `C:\UE-Template-Staging\ProjectArcana\ProjectArcana.uproject`.
6. Select **Create** and wait for shaders and assets to finish preparing. Do not initialize another Git repository in staging.
7. Close Unreal before copying the project.
8. Copy `ProjectArcana.uproject`, `Config`, and `Content` from the generated project into `C:\Dev\ProjectArcana`. Merge `Content` so both the template assets and `Content\Arcana` remain. Also copy `Source`, project `Plugins`, `Platforms`, or `Build` if the wizard generated them. A Blueprint-only template normally has no `Source` folder.
9. Do not copy generated `Binaries`, `DerivedDataCache`, `Intermediate`, `Saved`, or staging Git metadata. Preserve the checkout's `.git`, `.gitignore`, `.gitattributes`, README, and docs. Review filename collisions rather than blindly overwriting.
10. Double-click **C:\Dev\ProjectArcana\ProjectArcana.uproject** and use **Unreal Engine 5.5.4**. If multiple engines are installed and the file opens in the wrong one, close it and use **Browse** in the 5.5.4 project browser to select this `.uproject`. Do not convert it to a newer engine. All further edits belong in this checkout; do not keep editing the staging copy.

Expected result: the real Third Person template opens from the Git checkout. This guide deliberately does not supply a pretend `.uproject` or text files labeled as Blueprint `.uasset` files.

## 5. Use conservative graphics and save the test map

1. In the editor toolbar, open **Settings → Engine Scalability Settings** and start with **Medium**. If performance is poor, try **Low**. On some versions, scalability appears under the viewport's settings menu.
2. Open **Edit → Project Settings → Engine → Rendering**. For this initial test, leave hardware ray tracing disabled. If the template enabled expensive features, set **Dynamic Global Illumination Method** to **None**, **Reflection Method** to **Screen Space**, and **Shadow Map Method** to **Shadow Maps**, where those controls exist. Restart the editor if it requests it. Confirm the exact options against your installed version before changing unfamiliar settings.
3. Stay with the template's supported graphics API initially. An older GPU may need DirectX 11; diagnose an actual launch/rendering error before changing the API.
4. Open the **Content Drawer** (commonly **Ctrl+Space**). Under **Content → Arcana → World**, create a **Maps** folder if it is not already visible.
5. With the template level open, use **File → Save Current Level As** (or **Save Level As**) and save it in **Content/Arcana/World/Maps** as **L_FirstDuel_Courtyard**. Keep the template's floor and obstacles for now.
6. Open **Edit → Project Settings → Project → Maps & Modes**. Set **Editor Startup Map** and **Game Default Map** to **L_FirstDuel_Courtyard**. Retain the Third Person template's default GameMode; it selects the playable character and controller. If a World Settings GameMode override exists, retain the template's working setting.
7. Select **File → Save All**. Restart the project and confirm your saved map opens.

No external art is required. Start with primitive geometry and template assets. Prefer small textures (about 1K for ordinary placeholder props) when original art is introduced; select higher budgets only after measuring the PC. Do not enable advanced lighting or purchase assets to pass this test.

## 6. Run the required movement test

1. Open **L_FirstDuel_Courtyard**, then click **Play** in the toolbar. Use the dropdown beside Play to choose **Selected Viewport** if necessary.
2. Click inside the game viewport. Press **W/A/S/D** to move, move the mouse to turn the camera, and press **Space** to jump.
3. Verify the character moves on the ground, collides with an obstacle, jumps, and lands. Check that the camera follows correctly.
4. Press **Esc** to stop. Run Play again and confirm the character spawns and moves again.
5. Close and reopen `ProjectArcana.uproject`; confirm the saved map opens and repeat the test.
6. Open **Window → Developer Tools → Output Log** (in some versions **Window → Output Log**). Record any errors. Fill in the engine version, PC details, and test results in `docs/STATUS.md`. Mark a check passed only after you observe it.

If movement fails: click the viewport to focus it; check you selected **Play**, not **Simulate**; confirm a **Player Start** exists above the floor; and check **Maps & Modes** and **Window → World Settings** still use the template's GameMode rather than an empty one. If the screen is black, reopen the saved template map and check the Output Log. If the editor is slow, finish shader preparation and lower scalability before adding content.

Passing this test completes the initial playable-character check. It does not validate combat, a packaged Windows build, offline launch, or multiplayer.

## 7. Review and upload the first working project

From PowerShell in `C:\Dev\ProjectArcana`, before adding files:

```powershell
git status --short --branch
git check-attr filter -- Content/Arcana/World/Maps/L_FirstDuel_Courtyard.umap
git check-ignore -- Saved/Arcana.log Intermediate/example.tmp
```

The map's filter should be `lfs`. `Saved` and `Intermediate` should be ignored. Authored `Content`, `Config`, `.uproject`, docs, and both Git configuration files must be included; generated caches must not be included. Keep the Unreal Engine installation outside the repository.

Then run:

```powershell
git add .
git diff --cached --stat
git lfs ls-files
git status --short
```

Review the staged files before committing. Unreal maps and assets should appear in `git lfs ls-files`. If caches or unexpected files appear, stop and adjust what is staged. Git LFS uses GitHub storage/bandwidth allowances; stay within the free allowance and do not enable paid usage. Do not upload large asset packs for this initial test.

From the cloned `main` branch, commit the generated Unreal project on top of the existing setup files:

```powershell
git commit -m "Set up Project Arcana third-person prototype"
git push -u origin main
```

If the commit fails with **Author identity unknown**, set your identity for this repository, replacing these examples with the name and email you want on GitHub, then rerun the commit:

```powershell
git config user.name "Your Name"
git config user.email "your-address@example.com"
```

If another branch or commit has appeared on the remote since setup began, fetch and inspect it before pushing. Never force-push to resolve that situation. In GitHub's browser view, verify `main` contains `ProjectArcana.uproject`, `Config`, the content, and the documentation. A successful push is separate from the editor movement check.

## 8. Start combat only after Phase 0 passes

Next task: create a Blueprint spell definition structure and one turn-state controller that supports **Pass** and one damage spell without animation dependencies. See `ARCHITECTURE.md` for the boundary between rules and presentation. Add tests for command validation, resource spending, and phase transitions before expanding to six spells. Keep enemy encounters, UI polish, progression, and multiplayer out of that first combat task.

## Official references

These are official reference locations. Live documentation retrieval was blocked by the cloud proxy (403), so the links and version-sensitive menu names were not verified against a current release in this session.

- [Epic hardware and software specifications](https://dev.epicgames.com/documentation/en-us/unreal-engine/hardware-and-software-specifications-for-unreal-engine)
- [Install Unreal Engine](https://dev.epicgames.com/documentation/en-us/unreal-engine/installing-unreal-engine)
- [Create a project](https://dev.epicgames.com/documentation/en-us/unreal-engine/creating-a-new-project-in-unreal-engine)
- [Third Person template](https://dev.epicgames.com/documentation/en-us/unreal-engine/third-person-template-in-unreal-engine)
- [Scalability reference](https://dev.epicgames.com/documentation/en-us/unreal-engine/scalability-reference-for-unreal-engine)
- [Unreal directory structure](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-directory-structure)
- [Git LFS](https://git-lfs.com/)
