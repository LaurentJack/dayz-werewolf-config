# DayZ Werewolf – configuration serveur

*[English version](README.md)*

Fichiers de configuration pour le mod DayZ **Werewolf** (lycanthropie) : une morsure de loup, ou un
coup de griffes d'un loup-garou, transmet la malédiction. Après une incubation, le joueur se
transforme la nuit (tête de loup, mains et pieds griffus, fourrure sur la peau) et redevient humain
à l'aube.

Ce dépôt ne contient que la configuration, pas le mod lui-même.

| Fichier | Rôle | Où le placer |
|---|---|---|
| `WerewolfSettings.json` | Tous les réglages du mod, aux valeurs par défaut | `<profiles>/Werewolf/WerewolfSettings.json` |
| `serverDZ.fr.cfg` | Exemple de configuration du serveur DayZ (commentaires en français) | à la racine du serveur, renommé `serverDZ.cfg` |
| `serverDZ.cfg` | Le même fichier avec des commentaires en anglais | à la racine du serveur |

## Communauté

Questions, signalement de bugs et nouveautés : [Discord](https://discord.gg/6ESyZDXsAx)

## Installation

1. Charger le mod côté serveur et côté client : `-mod=@Werewolf`.
2. Copier `WerewolfSettings.json` dans le dossier `Werewolf` du dossier de profils du serveur
   (celui passé à `-profiles=`). Si le fichier est absent, le mod le crée lui-même avec ces mêmes
   valeurs au premier démarrage.
3. Adapter `serverDZ.fr.cfg` (à renommer `serverDZ.cfg`) : nom du serveur, **mot de passe administrateur** (vide dans l'exemple),
   nombre de joueurs, mission.
4. Redémarrer le serveur après chaque modification du JSON : il n'est lu qu'au démarrage.

Un réglage absent du JSON garde sa valeur par défaut. Les durées sont en secondes réelles.

Les textes du mod en jeu sont traduits dans les 13 langues de DayZ.

## Réglages (`WerewolfSettings.json`)

### Contamination
| Réglage | Défaut | Rôle |
|---|---|---|
| `WolfBiteChance` | 0.10 | Chance par morsure de loup (0 à 1) |
| `WerewolfBiteChance` | 0.50 | Chance par coup d'un loup-garou transformé |
| `ContagionBareHandsOnly` | true | Seules les griffes contaminent, pas un coup porté avec une arme |
| `ExtraWolfClassNames` | [] | Classes d'animaux moddés qui contaminent comme un loup |

### Incubation et remède
| Réglage | Défaut | Rôle |
|---|---|---|
| `IncubationDuration` | 3600 | Durée entre la morsure et la malédiction installée |
| `IncubationHalfwayWarning` | true | Message à mi-incubation |
| `IncubationPainIntervalMin` / `Max` | 90 / 200 | Intervalle aléatoire entre deux douleurs légères |
| `AconiteCuresCursed` | false | Le sérum d'aconit soigne aussi après l'incubation, même transformé |
| `AconiteCureDelay` | 45 | Délai entre l'injection et la guérison (0 = instantané) |
| `AconitePainInterval` | 8 | Intervalle entre deux accès de douleur pendant que le sérum agit |
| `AconitePainShock` | 8 | Choc perdu à chaque accès (100 = inconscient) |

Le sérum est l'objet `WerewolfAconiteSerum` ; pour le faire apparaître dans le butin, l'ajouter au
`types.xml` de la mission.

### Quelles nuits, quelles heures
| Réglage | Défaut | Rôle |
|---|---|---|
| `TransformMode` | 0 | 0 = chaque nuit, 1 = pleine lune seulement, 2 = une nuit sur N |
| `FullMoonMinIllumination` | 0.95 | Mode 1 : part éclairée minimale de la lune du jeu (0.95 ≈ 4 nuits par cycle, 0.99 ≈ 1 à 2) |
| `TransformEveryNights` | 3 | Mode 2 : une nuit sur N, comptée sur le calendrier du jeu |
| `NightMode` | 0 | 0 = nuit du moteur (lever/coucher du soleil de la carte), 1 = heures fixes |
| `NightStartHour` / `NightEndHour` | 21 / 5 | Heures de jeu utilisées si `NightMode` = 1 |

En mode pleine lune, le mod lit **la lune du jeu elle-même**, celle que les joueurs voient dans le
ciel : la transformation commence quand il fait nuit et que cette lune est levée et assez pleine,
et se termine à son coucher ou à l'aube. Le calendrier lunaire de DayZ n'est pas celui du vrai ciel.
La date du serveur doit avancer : voir `serverTimePersistent` et la remarque sur l'`init.c` dans
`serverDZ.fr.cfg`.

### Transformation
| Réglage | Défaut | Rôle |
|---|---|---|
| `ShiftDuration` | 25 | Durée de la transformation (douleur) après la tombée de la nuit |
| `ShiftPainIntervalMin` / `Max` | 4 / 8 | Intervalle entre deux grognements/douleurs pendant la transformation |
| `ShiftPainShock` | 5 | Choc perdu à chaque accès |
| `CanUseHands` | true | Le loup-garou peut tenir des objets ; false = griffes seules, l'objet en main est lâché |
| `FurOnSkin` | true | Fourrure sur la peau nue et les parties découvertes des vêtements |
| `TransformOutfit` | tête, mains, pieds | Pièces posées à la transformation |
| `FreedSlots` | Headgear, Mask, Eyewear, Gloves, Feet | Emplacements vidés (affaires rangées dans l'inventaire) et bloqués pendant la nuit |

### Pouvoirs du loup-garou
| Réglage | Défaut | Rôle |
|---|---|---|
| `ClawDamage` | 30 | Santé retirée en plus à chaque coup à mains nues (un coup de poing fait 10) |
| `ClawShock` | 25 | Choc en plus, sur les joueurs seulement |
| `ClawBleedChance` | 0.5 | Chance de faire saigner un joueur |
| `DamageResistance` | 0.5 | Part des dégâts reçus annulée (0 à 0.95) |
| `HealthRegenPerSec` | 0.5 | Santé régénérée par seconde (max 100) |
| `BloodRegenPerSec` | 10 | Sang régénéré par seconde (max 5000) |
| `StopBleeding` | true | Les saignements se referment |
| `StaminaDepletion` | 0.3 | Consommation d'endurance (< 1 = moins) |
| `StaminaRecovery` | 2.0 | Récupération d'endurance (> 1 = plus vite) |
| `EnergyDrainPerSec` | 0.15 | Faim supplémentaire par seconde |
| `NightVision` | true | Vision de nuit comme en plein jour |
| `NightVisionExposure` | 4.5 | Luminosité ajoutée la nuit (chaque point la double ; 7 = lunettes de vision nocturne du jeu) |
| `SprintSpeedBonus` | 0 | m/s ajoutés au sprint. Expérimental, saccade en jeu : laisser à 0 |

### Retour à l'humain, messages et sons
| Réglage | Défaut | Rôle |
|---|---|---|
| `RevertShock` | 60 | Choc à l'aube (100 = inconscient) |
| `RevertEnergyLoss` | 300 | Faim à l'aube |
| `ShowNotifications` | true | Messages à l'écran |
| `EnableSounds` | true | Grognements entendus par les joueurs proches |
| `GrowlIntervalMin` / `Max` | 40 / 90 | Intervalle aléatoire entre deux grognements en loup-garou |

### Tests (laisser à false / 0 sur un serveur public)
| Réglage | Rôle |
|---|---|
| `TestCurseOnConnect` | Maudit à la connexion, sans incubation |
| `TestForceNight` | Toujours nuit pour la malédiction, quel que soit `TransformMode` |
| `TestNightCycleSeconds` | Jour et nuit alternent, chacun de cette durée |
| `TestGiveAconiteOnConnect` | Un sérum donné à la connexion |
| `TestSpawnWolvesOnConnect` | Nombre de loups posés devant le joueur 15 s après la connexion |
| `TestSpawnDummyOnConnect` | Faux joueur immobile posé 3 m devant, pour essayer griffes et contagion |
| `DebugLog` | Changements d'état, coups et soins écrits dans `script_*.log` (préfixe `[Werewolf]`) |

## Vérifier que le mod est chargé

Trente secondes après son démarrage, le serveur écrit dans `script_*.log` une ligne du type :

```
[Werewolf] date 2026/10/17 17h, TransformMode 1, moon of the game lit 0.96 now (needs 0.95, 0 = below the horizon) -> the curse is awake
```

Elle indique la date du serveur, le mode choisi et si la malédiction est éveillée à cet instant.
