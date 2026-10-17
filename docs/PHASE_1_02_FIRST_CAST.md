# Phase 1, task 2: make the first cast work

The user reports that Phase 0 and the spell-structure/Data Table import checkpoint passed in the local Windows editor. Continue in that same **Unreal Engine 5.5.4** project. This guide has been reviewed as instructions; its Blueprint graphs have not been created or executed in this cloud machine.

This checkpoint implements one small combat rule: a valid Comet Pin cast spends **1 spark** and removes **30 enemy health**. Starting at health **100** and sparks **1**, the first cast must produce **70 / 0**. A second cast must fail and leave **70 / 0** unchanged. Also check a defeated target and a missing spell row.

There is no hand, turn order, enemy attack, button, animation, or damage applied to the visible mannequin yet. The controller stores the test enemy's health as a number. The existing character should continue moving normally.

## 1. Open the right project and create the controller

1. Open **C:\Dev\ProjectArcana\ProjectArcana.uproject** with **5.5.4**. Open **L_FirstDuel_Courtyard** if it is not already open.
2. Press **Ctrl+Space** for the Content Drawer. Find **Content → Arcana**.
3. Create a folder named **Combat** if it does not exist: right-click the Arcana folder, choose **New Folder**, and enter **Combat**.
4. Open **Combat**. Right-click empty space in the asset area, choose **Blueprint Class**, and select **Actor** as the parent class.
5. Name the asset **BP_CombatController**. Double-click it to open its Blueprint editor.

Keep **DT_Spells** and **ST_SpellDefinition** under **Content → Arcana → Data**. Do not create duplicate data assets.

## 2. Add three controller variables

In the Blueprint editor's **My Blueprint** panel, use **+** beside **Variables** to create these single-value variables. Select each variable to find its type and settings in **Details**.

| Variable name | Type | Default value | Instance Editable |
| --- | --- | --- | --- |
| EnemyHealth | Integer | 100 | Off |
| PlayerSparks | Integer | 1 | Off |
| bRunCastSelfTest | Boolean | False / unchecked | On |

1. Set the names and types first, then click **Compile**.
2. Select each variable again and enter its **Default Value** in Details. You may need to compile before the default-value field becomes available.
3. For **bRunCastSelfTest**, enable **Instance Editable**. Keep its default value unchecked. Unreal may display this variable as **Run Cast Self Test** in the level actor's Details panel.
4. Click **Compile**, then **Save**.

These are controller variables. Do not add them to ST_SpellDefinition; that structure describes a spell rather than changing combat state.

## 3. Create the rules function and its pins

1. In **My Blueprint**, click **+** beside **Functions**. Name the new function **TryCastSpell**.
2. Select the function in My Blueprint. In Details, leave **Pure** unchecked. This function changes state and needs white execution pins.
3. Under **Inputs**, click **+** and add **SpellRowName**, type **Name**.
4. Under **Outputs**, add **Success**, type **Boolean**, and **FailureReason**, type **String**.

The function graph should contain its entry node and a **Return Node**. The entry has a white execution output and the SpellRowName data output. Return nodes have a white execution input and the two result values.

You will need **five Return Nodes**: one success and four failures. Select the existing Return Node and press **Ctrl+W** to duplicate it as needed. Each copy belongs to a separate exit path; do not connect Return Nodes to one another.

Set their unconnected result-pin values as follows:

| Exit path | Success | FailureReason |
| --- | --- | --- |
| Target health is zero or below | Unchecked / False | TargetDefeated |
| Data Table row does not exist | Unchecked / False | MissingSpellRow |
| Row fields are invalid or unsupported | Unchecked / False | InvalidSpellDefinition |
| Not enough sparks | Unchecked / False | InsufficientResources |
| Cast accepted | Checked / True | Empty string: leave the field blank |

The exit-path descriptions identify the different Return Nodes. You do not need to rename the nodes themselves; use their FailureReason fields to tell them apart.

Do not add **Print String**, widgets, Delay, animation, or **Apply Damage** nodes inside this function. It returns its result to the caller; the caller handles display.

## 4. Reject a defeated target

In these instructions, **white wires control execution**. Colored wires carry values. A **Get** variable node reads a value; a **Set** node changes it.

1. Drag **EnemyHealth** from My Blueprint into the function graph. Choose **Get EnemyHealth**.
2. Drag from its integer output pin and search for **>**. Choose the integer **Greater** comparison. Set the other integer input to **0**. The comparison's Boolean output now means EnemyHealth is greater than zero.
3. Right-click empty space and add a **Branch**.
4. Connect the function entry's **white execution output → Branch execution input**.
5. Connect the comparison's **Boolean output → Branch Condition**.
6. Connect **Branch False → TargetDefeated Return Node**.

Leave Branch True available for the next section. No state has changed on this failure path.

## 5. Read the requested spell from DT_Spells

1. Right-click empty space and search for **Get Data Table Row**. Add that node.
2. On its **Data Table** asset selector, choose the existing **DT_Spells** asset. Set this asset before working with Out Row; Unreal uses its row structure to determine the output type.
3. Connect the previous **Branch True → Get Data Table Row execution input**.
4. Connect the function entry's **SpellRowName → Get Data Table Row Row Name** input. The function receives the row name from its caller rather than hardcoding it here.
5. Connect **Row Not Found → MissingSpellRow Return Node**.
6. Drag from **Out Row** and choose **Break ST_SpellDefinition**, or the matching **Break** option for that structure. Expand its pins if necessary so you can see SpellId, ResourceCost, BaseDamage, and AccuracyPercent.

The **Break** node has no white execution pins. It only exposes the fields of the row; do not look for an execution wire to connect to it. Use **Row Found** as the white execution path for the next validation.

If the compatible Break node is missing, first check that Get Data Table Row selects the correct DT_Spells and that the table uses ST_SpellDefinition. Do not substitute an unrelated structure.

## 6. Validate the row before spending anything

Create these four Boolean comparisons. Drag from each field's colored output pin before searching for the comparison so Unreal chooses the correct type.

| Comparison | Connections / literal value |
| --- | --- |
| SpellId equals the requested row name | Break node **SpellId → Equal (Name)** first input; function entry **SpellRowName →** second input |
| ResourceCost is not negative | Break node **ResourceCost → >= (Integer)** first input; second input **0** |
| BaseDamage is not negative | Break node **BaseDamage → >= (Integer)** first input; second input **0** |
| AccuracyPercent is exactly 100 | Break node **AccuracyPercent → == (Integer)** first input; second input **100** |

1. Drag from a comparison's Boolean output and add **AND Boolean**. Use **Add Pin** on the AND node until it has **four inputs**.
2. Connect one comparison result to each AND input. All four inputs must have wires; an unused Boolean input would default to false.
3. Add another **Branch**.
4. Connect **Get Data Table Row Row Found → this Branch execution input**.
5. Connect **AND output → this Branch Condition**.
6. Connect **Branch False → InvalidSpellDefinition Return Node**.

The initial resolver supports only a guaranteed direct-damage cast. An accuracy other than 100 is deliberately rejected until a future task implements controlled randomness. This prevents the controller from silently ignoring an accuracy field.

## 7. Check the cost, then commit the cast

1. Drag **PlayerSparks** into the graph as **Get PlayerSparks**.
2. Drag from its integer output and create **>= (Integer)**. Connect **PlayerSparks → first input** and **Break node ResourceCost → second input**.
3. Add a **Branch**. Connect the previous validation **Branch True → this Branch execution input**.
4. Connect the affordability comparison's **Boolean output → Condition**.
5. Connect **Branch False → InsufficientResources Return Node**.

Only this last Branch's **True** path is allowed to reach setters:

1. Drag from a **Get PlayerSparks** output and create an integer **Subtract** node. Connect **PlayerSparks → first input** and **Break node ResourceCost → second input**.
2. Drag **PlayerSparks** from My Blueprint into the graph and choose **Set PlayerSparks**. Connect the subtraction's integer result to its value input.
3. Connect the affordability **Branch True → Set PlayerSparks execution input**.
4. From a **Get EnemyHealth** output, create an integer **Subtract** node. Connect **EnemyHealth → first input** and **Break node BaseDamage → second input**.
5. From that subtraction's result, create **Max (Integer)**. Connect the subtraction result to one input and set the other input to **0**. This prevents health from dropping below zero.
6. Drag **EnemyHealth** into the graph and choose **Set EnemyHealth**. Connect the Max result to its value input.
7. Connect **Set PlayerSparks white execution output → Set EnemyHealth execution input**.
8. Connect **Set EnemyHealth white execution output → the success Return Node**. Verify this Return Node has **Success checked** and an **empty FailureReason**.
9. Click **Compile** and **Save**. Resolve any compiler errors before continuing.

The order is now: validate target, find row, validate fields, validate resources, spend resources, reduce health, return success. Every rejected path returns before either setter. Do not add a Delay or other latent operation between validation and these state changes.

## 8. Build a temporary test harness in the Event Graph

This harness will call the actual function and compare its actual results with the expected numbers. A printed PASS must come from a successful comparison rather than an unconditional message.

### Add the test gate and Sequence

1. Click **Event Graph** in My Blueprint. Find **Event BeginPlay**. If it is absent, right-click empty space and add it.
2. Add a **Branch** and drag **bRunCastSelfTest** into the graph as a **Get** node.
3. Connect **Event BeginPlay → Branch execution input** and **Get bRunCastSelfTest → Condition**.
4. Add a **Sequence** node. Connect **Branch True → Sequence**. Leave Branch False unconnected.
5. Use **Add Pin** on Sequence until you have six outputs, **Then 0** through **Then 5**.

A Sequence runs each connected path in order. These calls are synchronous, so one path finishes before the next starts. Do not connect a test path back into Sequence or use Delay nodes.

### Then 0: establish the starting state

1. Create **Set EnemyHealth**, with value **100**, and **Set PlayerSparks**, with value **1**.
2. Wire **Then 0 → Set EnemyHealth → Set PlayerSparks** using the white execution pins.

### Then 1: verify the accepted cast

1. Drag **TryCastSpell** from the Functions list into the Event Graph to create a call node. Set its **Spell Row Name** input to **astral_comet_pin**.
2. Connect **Then 1 → TryCastSpell execution input**.
3. Add the four checks below. For integer and String comparisons, drag from the value's output pin and search for **==** to obtain the right type.

| Check | How to obtain its Boolean result |
| --- | --- |
| Function succeeded | Use the call's **Success** output directly |
| EnemyHealth is 70 | **Get EnemyHealth → == (Integer)**, other input **70** |
| PlayerSparks is 0 | **Get PlayerSparks → == (Integer)**, other input **0** |
| No failure reason | Call's **FailureReason → == (String)**, other input empty |

4. Combine those four results in a four-input **AND Boolean** node.
5. Add a **Branch**. Connect the call's **white execution output → Branch**, and **AND output → Condition**. Reading health and sparks here occurs after the function call completes.
6. Add two **Print String** nodes. Connect **Branch True →** the first, with In String **ARCANA TEST 1 PASS: accepted cast, HP 70, sparks 0**. Connect **Branch False →** the second, with In String **ARCANA TEST 1 FAIL: accepted-cast check**.
7. Expand each Print String node's advanced pins if needed. Ensure **Print to Log** is checked. You may also enable **Print to Screen** and set **Duration** to **10**.

### Then 2: verify that a second cast cannot spend unavailable sparks

1. Create another **TryCastSpell** call with **Spell Row Name = astral_comet_pin**. Connect **Then 2 → this call**. Do not reset health or sparks on this path.
2. Build four checks and combine them with another four-input AND:

| Check | Expected result |
| --- | --- |
| Call Success is false | Drag from **Success** and create **NOT Boolean**; use its output |
| EnemyHealth is unchanged | **Get EnemyHealth == 70** |
| PlayerSparks is unchanged | **Get PlayerSparks == 0** |
| Correct rejection | Call **FailureReason == InsufficientResources** using a String comparison |

3. Connect **call execution output → a new Branch**, with **AND output → Condition**.
4. Connect True to a Print String containing **ARCANA TEST 2 PASS: insufficient resources, state unchanged**. Connect False to a separate Print String containing **ARCANA TEST 2 FAIL: insufficient-resources check**. Enable Print to Log on both.

### Then 3: verify that a defeated target cannot be hit

1. Create setters and a new TryCastSpell call. Wire **Then 3 → Set EnemyHealth (0) → Set PlayerSparks (1) → TryCastSpell**.
2. Set this call's **Spell Row Name = astral_comet_pin**.
3. Combine these four checks in a four-input AND: **NOT Success**, **EnemyHealth == 0**, **PlayerSparks == 1**, and **FailureReason == TargetDefeated**.
4. Connect **call execution output → a new Branch**, with **AND output → Condition**.
5. True prints **ARCANA TEST 3 PASS: defeated target, state unchanged**. False prints **ARCANA TEST 3 FAIL: defeated-target check**. Enable Print to Log on both.

### Then 4: verify that an unknown row cannot spend sparks

1. Confirm that DT_Spells has no row called **missing_spell**. Do not add one for this test.
2. Wire **Then 4 → Set EnemyHealth (100) → Set PlayerSparks (1) → a new TryCastSpell call**.
3. Set this call's **Spell Row Name = missing_spell**.
4. Combine these four checks in a four-input AND: **NOT Success**, **EnemyHealth == 100**, **PlayerSparks == 1**, and **FailureReason == MissingSpellRow**.
5. Connect **call execution output → a new Branch**, with **AND output → Condition**.
6. True prints **ARCANA TEST 4 PASS: unknown spell, state unchanged**. False prints **ARCANA TEST 4 FAIL: unknown-spell check**. Enable Print to Log on both.

The deliberate missing-row lookup may also produce an engine Data Table warning. Distinguish that expected lookup warning from an ARCANA TEST failure or a Blueprint runtime error.

### Then 5: leave a clean starting state

Wire **Then 5 → Set EnemyHealth (100) → Set PlayerSparks (1)**. These setters reset the temporary tests' state for subsequent work; they do not indicate that any test passed.

Click **Compile** and **Save**. Keep all Print String nodes in this test harness rather than moving them into TryCastSpell.

## 9. Place one controller and run the checks

1. Return to the level editor and verify the open map is **L_FirstDuel_Courtyard**.
2. In **Content → Arcana → Combat**, drag **BP_CombatController** into the level viewport. It does not need a mesh. Place **exactly one** instance; inspect the Outliner for duplicates.
3. Select that actor in the Outliner. In its **Details** panel, find **Run Cast Self Test** and check it. This enables only this actor instance; leave the Blueprint class default false.
4. Choose **File → Save All**.
5. Open **Output Log**, usually available on the bottom editor bar. You can filter its text by **ARCANA TEST**.
6. Press **Play**. Read the four logged results in order:

| Test | Required result |
| --- | --- |
| 1: normal cast | PASS; health becomes 70 and sparks become 0 |
| 2: no sparks left | PASS; health stays 70 and sparks stay 0 |
| 3: target already defeated | PASS; health stays 0 and sparks stay 1 |
| 4: row does not exist | PASS; health stays 100 and sparks stay 1 |

7. Press **Escape** to stop Play. Run Play again and confirm all four PASS results repeat. On-screen messages may be stacked in reverse display order; use the Output Log for their execution order.
8. If any check prints FAIL, stop here and inspect that path. Do not bypass it or change the expected values to make it pass. You can add a Blueprint breakpoint to that test's Branch and inspect the actual Success, FailureReason, EnemyHealth, and PlayerSparks values during Play.
9. After both runs pass, stop Play, select the controller actor, **uncheck Run Cast Self Test**, and **Save All**. The harness remains available for an explicit future test rather than silently changing combat state every time the map starts.

## 10. Troubleshoot the graph rather than guessing

- **Nothing prints:** verify one controller is placed in the open map, its instance checkbox is enabled, BeginPlay reaches the gate, and Print to Log is checked. The Blueprint class default being false is intentional.
- **A row or structure cannot be found:** verify the existing DT_Spells selects ST_SpellDefinition and includes the exact row key **astral_comet_pin**. SpellId should have that same Name value.
- **A function pin or variable has the wrong type:** the row input and SpellId are **Name**; Success is **Boolean**; FailureReason is **String**; health, sparks, cost, damage, and accuracy are **Integer**. Do not convert the numbers to String to compare them.
- **The first test fails:** reopen DT_Spells and verify cost **1**, damage **30**, accuracy **100**, and SpellId **astral_comet_pin**. Check the subtraction input order and the success Return Node's checkbox.
- **The second test fails:** do not add a reset before Then 2. Inspect the affordability Branch; only True may reach the setters. All four-input AND nodes must have all four inputs connected.
- **A rejection changes health or sparks:** find any state setter before the final affordability Branch. Move it onto the accepted path and rerun all tests.
- **Four sets of results repeat unexpectedly:** check for duplicate actors or additional calls to the harness. One placed instance should produce one set per Play session.
- **No animation or visible health bar appears:** expected at this checkpoint. The tests exercise rules stored on the controller, not presentation or the mannequin's health.

## 11. Save and share the actual result

After the checks pass, save the Blueprint and level. Record the actual local result in the project status notes. Keep **BP_CombatController.uasset**, any saved map changes, and any authored external actor/object files under source control using the existing Git LFS rules. The Blueprint must be created and tested in Unreal; this Markdown guide does not substitute for it.

Close Unreal before uploading. Review **git status --short --branch**, **git diff --cached --stat**, and **git lfs ls-files** before committing. Include authored files in **Content/__ExternalActors__** and **Content/__ExternalObjects__** when changed; they can contain level actors rather than disposable caches. Continue excluding Saved, Intermediate, Binaries, and DerivedDataCache. Leave unrelated unfamiliar files unstaged. Do not overwrite local work or force-push to obtain these instructions.

The checkpoint is complete only when the real graph compiles and the four checks pass twice in the local editor. The next task will add explicit combat phases, a Pass command, resource generation, and a small enemy turn around this validated cast function. Full cards and the six-spell duel follow those rules.
