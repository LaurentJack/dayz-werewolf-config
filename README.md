# DayZ Werewolf – server configuration

*[Version française](README.fr.md)*

Configuration files for the DayZ **Werewolf** mod (lycanthropy): a wolf bite, or a claw hit from a
werewolf, passes on the curse. After an incubation period the player transforms at night (wolf head,
clawed hands and feet, fur on the skin) and turns human again at dawn.

This repository only holds the configuration, not the mod itself.

| File | Purpose | Where it goes |
|---|---|---|
| `WerewolfSettings.json` | Every setting of the mod, at its default value | `<profiles>/Werewolf/WerewolfSettings.json` |
| `serverDZ.cfg` | Example DayZ server configuration (English comments) | server root |
| `serverDZ.fr.cfg` | Same file with French comments | server root, renamed `serverDZ.cfg` |

## Installation

1. Load the mod on the server and on the clients: `-mod=@Werewolf`.
2. Copy `WerewolfSettings.json` into the `Werewolf` folder of the server profile folder (the one
   given to `-profiles=`). If the file is missing, the mod creates it with these same values on the
   first start.
3. Adapt `serverDZ.cfg`: server name, **admin password** (empty in the example), player count,
   mission.
4. Restart the server after every change to the JSON: it is only read at startup.

A setting missing from the JSON keeps its default value. Durations are in real seconds.

The mod's in-game texts are translated into the 13 languages of DayZ.

## Settings (`WerewolfSettings.json`)

### Contagion
| Setting | Default | Purpose |
|---|---|---|
| `WolfBiteChance` | 0.10 | Chance per wolf bite (0 to 1) |
| `WerewolfBiteChance` | 0.50 | Chance per hit from a transformed werewolf |
| `ContagionBareHandsOnly` | true | Only claws spread the curse, not a hit with a weapon |
| `ExtraWolfClassNames` | [] | Modded animal classes that spread the curse like a wolf |

### Incubation and cure
| Setting | Default | Purpose |
|---|---|---|
| `IncubationDuration` | 3600 | Time between the bite and the curse taking hold |
| `IncubationHalfwayWarning` | true | Message halfway through the incubation |
| `IncubationPainIntervalMin` / `Max` | 90 / 200 | Random interval between two bouts of light pain |
| `AconiteCuresCursed` | false | The aconite serum also cures after the incubation, even when transformed |
| `AconiteCureDelay` | 45 | Delay between the injection and the cure (0 = instant) |
| `AconitePainInterval` | 8 | Interval between two bouts of pain while the serum acts |
| `AconitePainShock` | 8 | Shock lost at each bout (100 = unconscious) |

The serum is the `WerewolfAconiteSerum` item; add it to the mission's `types.xml` to make it spawn as
loot.

### Which nights, which hours
| Setting | Default | Purpose |
|---|---|---|
| `TransformMode` | 0 | 0 = every night, 1 = full moon only, 2 = one night out of N |
| `FullMoonMinIllumination` | 0.95 | Mode 1: minimum lit fraction of the in-game moon (0.95 ≈ 4 nights per cycle, 0.99 ≈ 1 to 2) |
| `TransformEveryNights` | 3 | Mode 2: one night out of N, counted on the in-game calendar |
| `NightMode` | 0 | 0 = engine night (sunrise/sunset of the map), 1 = fixed hours |
| `NightStartHour` / `NightEndHour` | 21 / 5 | In-game hours used when `NightMode` = 1 |

In full-moon mode the mod reads **the moon of the game itself**, the one players see in the sky: the
transformation starts when it is night and that moon is up and full enough, and ends when it sets or
at dawn. DayZ's lunar calendar is not the one of the real sky. The server date has to move on: see
`serverTimePersistent` and the note about `init.c` in `serverDZ.cfg`.

### Transformation
| Setting | Default | Purpose |
|---|---|---|
| `ShiftDuration` | 25 | Length of the change (pain) after nightfall |
| `ShiftPainIntervalMin` / `Max` | 4 / 8 | Interval between two growls / bouts of pain during the change |
| `ShiftPainShock` | 5 | Shock lost at each bout |
| `CanUseHands` | true | The werewolf can hold items; false = claws only, the item in hands is dropped |
| `FurOnSkin` | true | Fur on bare skin and on the uncovered parts of clothes |
| `TransformOutfit` | head, hands, feet | Pieces put on at the transformation |
| `FreedSlots` | Headgear, Mask, Eyewear, Gloves, Feet | Slots emptied (gear stored in the inventory) and blocked during the night |

### Werewolf powers
| Setting | Default | Purpose |
|---|---|---|
| `ClawDamage` | 30 | Extra health removed by every bare-hand hit (a punch does 10) |
| `ClawShock` | 25 | Extra shock, on players only |
| `ClawBleedChance` | 0.5 | Chance to make a player bleed |
| `DamageResistance` | 0.5 | Share of received damage cancelled (0 to 0.95) |
| `HealthRegenPerSec` | 0.5 | Health regenerated per second (max 100) |
| `BloodRegenPerSec` | 10 | Blood regenerated per second (max 5000) |
| `StopBleeding` | true | Bleeding wounds close |
| `StaminaDepletion` | 0.3 | Stamina consumption (< 1 = less) |
| `StaminaRecovery` | 2.0 | Stamina recovery (> 1 = faster) |
| `EnergyDrainPerSec` | 0.15 | Extra hunger per second |
| `NightVision` | true | Sees at night as in daylight |
| `NightVisionExposure` | 7.0 | Brightness added at night (7 = the game's night-vision goggles) |
| `SprintSpeedBonus` | 0 | m/s added to the sprint. Experimental, stutters in game: leave at 0 |

### Back to human, messages and sounds
| Setting | Default | Purpose |
|---|---|---|
| `RevertShock` | 60 | Shock at dawn (100 = unconscious) |
| `RevertEnergyLoss` | 300 | Hunger at dawn |
| `ShowNotifications` | true | On-screen messages |
| `EnableSounds` | true | Growls heard by nearby players |
| `GrowlIntervalMin` / `Max` | 40 / 90 | Random interval between two growls in werewolf form |

### Testing (leave at false / 0 on a public server)
| Setting | Purpose |
|---|---|
| `TestCurseOnConnect` | Cursed on connect, without incubation |
| `TestForceNight` | Always night for the curse, whatever `TransformMode` says |
| `TestNightCycleSeconds` | Day and night alternate, each lasting this long |
| `TestGiveAconiteOnConnect` | One serum given on connect |
| `TestSpawnWolvesOnConnect` | Number of wolves placed in front of the player 15 s after connecting |
| `TestSpawnDummyOnConnect` | Motionless fake player placed 3 m ahead, to try claws and contagion |
| `DebugLog` | State changes, hits and cures written to `script_*.log` (prefix `[Werewolf]`) |

## Checking that the mod is loaded

Thirty seconds after it starts, the server writes a line like this to `script_*.log`:

```
[Werewolf] date 2026/10/17 17h, TransformMode 1, moon of the game lit 0.96 now (needs 0.95, 0 = below the horizon) -> the curse is awake
```

It gives the server date, the chosen mode and whether the curse is awake at that moment.
