# Phase 1, task 1: create the first spell definition

Use the existing Windows project in **Unreal Engine 5.5.4**. The user has reported Phase 0 successful; do not recreate the Third Person project.

This task creates real spell data in the editor. It does not yet cast a spell, deal damage, create a hand, or start a duel. The first spell is **Comet Pin**: eventually it will cost 1 spark and deal 30 damage. It starts at 100% accuracy so the first combat checks can be reproduced without random misses.

## 1. Locate the CSV source

The prepared source file is `SourceData/Spells/DT_Spells.csv` in this repository. Keep it outside `Content`: the editor must have the row structure before importing it.

Use the Raw download button on that file's GitHub page to save the actual CSV, or update the Windows checkout after preserving local changes. Do not overwrite the Windows project or reset it to obtain the CSV. The user has now reported this import checkpoint complete; these instructions remain a reference rather than a request to repeat it.

## 2. Create the spell structure

1. Open the checkout's `ProjectArcana.uproject` using **5.5.4**.
2. Press **Ctrl+Space** to open the **Content Drawer**. In its folder tree, open **Content → Arcana → Data**.
3. Right-click empty space in the asset area, then choose **Blueprints → Structure**. Name the new asset **ST_SpellDefinition** and press **Enter**.
4. Double-click **ST_SpellDefinition** to open it. Rename its initial variable **SpellId**. Change its variable type using the type dropdown to **Name**.
5. Select **Add Variable** (or **+ Variable**) for each remaining field. Use these exact member names and single-value types; do not make arrays:

| Member name | Variable type |
| --- | --- |
| SpellId | Name |
| DisplayName | String |
| ResourceCost | Integer |
| BaseDamage | Integer |
| AccuracyPercent | Integer |

6. Click **Save** in the structure editor, then close that tab. A Structure is a reusable collection of named fields; it is not a Blueprint Actor and has no Event Graph.

Do not create a structure member named `Name`. The CSV's first column, `Name`, supplies the Data Table row key automatically. `SpellId` is a separate stable gameplay identifier.

## 3. Import the first spell

1. Return to **Content → Arcana → Data** in the Content Drawer.
2. Click **Import** and select the actual **DT_Spells.csv** file from disk. Do not select a downloaded HTML page or a file named `.csv.txt`.
3. In the import dialog, choose **DataTable** as the import type and **ST_SpellDefinition** as the row structure, then confirm the import.
4. The new asset should be named **DT_Spells**. If the downloaded file had another name, rename the imported asset to **DT_Spells**.
5. Double-click **DT_Spells**. Select the **astral_comet_pin** row and confirm:

| Field | Expected value |
| --- | --- |
| Row name | astral_comet_pin |
| SpellId | astral_comet_pin |
| DisplayName | Comet Pin |
| ResourceCost | 1 |
| BaseDamage | 30 |
| AccuracyPercent | 100 |

6. Choose **File → Save All**. Close and reopen **DT_Spells**, then verify those values remain.

Expected real editor assets: `Content/Arcana/Data/ST_SpellDefinition.uasset` and `Content/Arcana/Data/DT_Spells.uasset`. The assistant cannot create or test those binary assets in this cloud machine; the CSV is their import source.

## 4. Resolve common import problems

- **ST_SpellDefinition is missing from the import dropdown:** save the structure first, then reopen the import dialog.
- **Missing or unrecognized columns:** compare all five member names and their types with the table above. Do not enable an option to ignore missing fields to conceal the mismatch.
- **The downloaded file contains a web page:** download the file's raw contents. The first line must be `Name,SpellId,DisplayName,ResourceCost,BaseDamage,AccuracyPercent`.
- **The data is correct but nothing happens in Play:** expected at this checkpoint. No controller consumes the data yet.

## 5. Report the actual result

Record the import and reopen check in `docs/STATUS.md` after they pass. Keep the CSV source and the two real `.uasset` files in source control, with Git LFS active for the binary assets. Do not claim spell execution based on a successful import.

Next gameplay task: follow [the first-cast Blueprint guide](PHASE_1_02_FIRST_CAST.md) to create a controller that reads this row and supports a valid cast and a rejected cast. Starting from enemy health 100 and player resources 1, a successful cast must produce health 70 and resources 0. A second cast with no resources must leave all state unchanged. Add Pass, explicit phases, and a simple enemy turn after that core transaction is correct, then expand to the full six-spell duel.
