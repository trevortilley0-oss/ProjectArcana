# Project Arcana: development boundaries

This is a design note for implementation. The user reports Phase 0 succeeded in the local Windows Unreal project. The shared checkout does not yet contain that project's binary assets. Combat and spells are not implemented. Begin Phase 1 with the spell-data checkpoint, then a small tested combat controller.

## Confirmed decisions

- Unreal Engine **5.5.4**, already installed on the reported Windows development PC. Do not upgrade during Phase 0. Hardware specifications are recorded in `STATUS.md`; actual editor performance remains unverified.
- Blueprints first. Add C++ only when a concrete architecture, testing, or networking need justifies it.
- Windows PC, single player, playable offline, and a $0 starting budget.
- Original stylized fantasy content. Publicly documented game mechanics can inform design; proprietary assets and source code cannot be reused.
- One player versus one enemy first. Keep room for more combatants without building networking or a multiplayer server.
- Built-in Unreal features and placeholder assets first; verify licenses before importing third-party content.

## Keep five responsibilities separate

| Responsibility | Owns | Does not decide |
| --- | --- | --- |
| Exploration | Character movement, interactions, encounter entry and return | Spell damage or battle outcomes |
| Combat rules | Battle state, legal commands, resources, draw/discard, effect resolution, victory | Animation timing, camera movement, widget contents |
| Content | Spell and enemy definitions, future quests and equipment | Mutable battle state |
| Presentation | Widgets, animation, effects, camera, sound | Whether an action succeeds or how much damage occurs |
| Persistence | Future local progression and unlock saves | Live UI or animation references |

Use an encounter controller as the first local authority for combat. Route UI input through validated commands so a future authority can replace that controller. Do not add replication, accounts, hosting, or services now.

## Minimal combat contract for Phase 1

1. **State:** combatant IDs, health, resources, deck/hand/discard card instances, active effects, current round/phase, and controlled random state. Identify a card instance separately from its spell definition so duplicate spells remain distinguishable.
2. **Command:** an actor ID, card instance ID, and target IDs, or an explicit pass. Validate the phase, actor, owned card, living/legal targets, and resource cost before accepting it. Rejected commands must leave state and random state unchanged.
3. **Resolution:** calculate the new state using combat data, accepted commands, and a seeded random stream. Use stable iteration and action order. Begin with integer values and explicit rounding rules; define rules before implementing them.
4. **Events:** return plain records such as `CardPlayed`, `ResourceSpent`, `DamageApplied`, `HealingApplied`, `EffectAdded`, and `BattleEnded`. Include IDs and resolved amounts, not widget or animation references.
5. **Presentation:** show those events in order. Accelerating or skipping an animation must leave the same final combat state.

Initially, determinism means reproducing a battle within the same implemented rules and engine version using the same starting state, seed, and commands. Cross-version or cross-platform determinism is not established.

### Planned state machine

`Initialize → RoundStart → Planning → Resolve → RoundEnd → RoundStart`

Transition to `Result` when victory or defeat is detected, then exit the encounter. Round start generates resources and draws eligible cards. Planning gathers the player's choice and the enemy's choice. Resolve processes accepted actions in a documented stable order, revalidating targets when their action resolves. Round end advances timed effects. Establish exact resource caps, card replacement, discard/refill, action ordering, simultaneous defeat, and status-duration rules in Phase 1 before testing them.

Keep state transitions independent of animation duration. The presentation can wait to display the next event while rules remain authoritative.

### Data-driven spell definitions

For the first checkpoint in UE 5.5.4, use a Blueprint Structure and Data Table with only ID, display name, resource cost, direct damage, and accuracy. The importable source is `SourceData/Spells/DT_Spells.csv`. This is a deliberately small initial schema, not the complete spell system. Extend the data model with discipline, success rules, target type, healing, reusable effects and durations, modifiers, upgrade/unlock/rarity data, presentation references, and AI evaluation values as their rules are implemented. Keep mutable health, remaining durations, and draw order in battle state.

## Six example spell functions — not implemented

These original names and numbers are provisional balancing examples for Phase 1, not finished content or verified mechanics. A resource is tentatively called a **spark**; its generation and cap remain to be specified.

| Example spell ID / name | Discipline | Example cost | Distinct function |
| --- | --- | --- | --- |
| `astral_comet_pin` / Comet Pin | Astral | 1 spark | Deal 30 direct damage to one enemy. |
| `astral_orbit_break` / Orbit Break | Astral | 2 sparks | Remove up to 1 stored spark from one enemy. |
| `verdant_dew_mend` / Dew Mend | Verdant | 1 spark | Restore up to 25 health to self, capped at maximum health. |
| `verdant_springthread` / Springthread | Verdant | 2 sparks | Restore up to 10 health at each of the next three round ends. |
| `runic_slate_aegis` / Slate Aegis | Runic | 1 spark | Grant a shield that absorbs up to 20 damage before health is lost. |
| `runic_ember_script` / Ember Script | Runic | 2 sparks | Apply 12 damage at each of the next three round ends. |

Use the same reusable resolution paths for direct and timed damage/healing. Specify stacking, refresh, resistance, shield interaction, accuracy, and duration timing before these become playable spells. The early prototype uses placeholders for visual effects and audio.

## Combat validation required later

Include invalid card plays, insufficient resources, invalid/defeated targets, shields, healing caps, damage/resistance, status expiration, deck exhaustion, victory/defeat, and phase transitions. Add a seed-and-command replay check and confirm skipped presentation yields the same outcome. Verify the playable loop in Unreal Editor; text files and isolated calculations cannot establish editor integration.
