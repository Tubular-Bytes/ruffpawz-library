# Changelog

Changes to the Ruff Pawz library, newest first. The format follows [Keep a Changelog](https://keepachangelog.com/).

The history has two lines. `v1.x` is the line that runs in production (`v1.1.0` is the current one there). `v2.x` is the item rework (`library-rework`), which needs engine 0.28.0 or later for the four stats, `k.yaml` and `mitigate`, and comes with a character wipe.

## [2.3.0] - 2026-10-22

Everything since `v2.2.0`: the biggest change of the rework so far.

### Changed

- **All numbers are scaled ×10** for finer tuning: base stats are now hp 1000, physical and magic attack 100, physical and magic defense 70, initiative 100, and `k.yaml` is 400 for both kinds. Flat modifiers (`op: add`), affix tier ranges and every constant in the skills are scaled too. Percentages, multipliers (`op: mul`), the `f` factors, chances, durations and cooldowns are not. A simulation of the library before and after showed the same results within noise.
- **Armors and weapons roll affixes** instead of fixed modifiers. Plate now rolls `physical_defense` (tier 3 or better) and `magic_defense` (up to tier 3) and keeps −20 initiative. Leather offers `physical_defense` and `initiative`. The robe and the ember robe offer `magic_defense` (and `initiative`). The sword lost its random 0–20 bonus and rolls `physical_attack` up to tier 3. The dagger rolls `physical_attack` or `initiative`.
- **Robe** now offers Second Wind and Guard (it was Mend and Guard). Second Wind is unretired for this.
- **Staves:** the elemental staves offer only their own skill (no Sweep). The plain staff keeps Chain Lightning and Sweep and has −10 physical attack. The sword offers Sweep and Power Strike (it was Sweep and Backstab).
- **Skills use `mitigate` more:** Arcane Bolt (180%, magic), Chain Lightning (80% of magic attack, `f` 0.8) and Firestorm (50%, magic, `f` 0.6) now count the target's magic defense. Arcane Bolt no longer deals its damage twice.
- **Balance of skills:** Backstab has cooldown 3 (was 2). Flurry hits for 100% of physical attack (was 80%). Frost Bolt rounds its damage like the other skills. Heals are lowered to cut stalemates: Mend 250, Mass Heal 125, Healing Potion 250 and Drain heals 125% of magic attack (300, 150, 400 and 150% after the ×10 scaling). Entangle deals 50% of magic attack per round and lowers initiative by 40.
- **Races:** the pup starts with Rage instead of Guard. Bat, dragon (hp ×1.6, magic defense +50), kitten (+10 physical attack, −80 magic defense) and the existing modifiers are rebalanced. Descriptions follow the numbers.
- **Classes:** the rogue has hp ×0.8, `physical_attack` ×1.3 and +30 initiative. The warrior has +200 hp.

### Added

- Classes: **gladiator** (Power Strike), **priest** (Mass Heal), **ranger** (Skillshot; hp ×0.6, +80 physical attack, +30 initiative), **warlock** (Arcane Bolt).
- Races: **rat** (Rend; hp ×0.8, +30 initiative, +30 physical attack, lower defenses, with a name generator) and **frog** (retired placeholder).
- Skills: **Bless**, **Chaos Bolt** (150% of magic attack), **Flame Bolt** and **Lightning Bolt** (70% of magic attack plus a damage-over-time effect of 30% for 3 rounds), **Entangle**, **Haste Potion**, **Mass Heal**, **Rage**, **Rend**, **Skillshot** (200% of physical attack, cooldown 5).
- Weapons: **chaos staff**, **earth staff**, **fire staff**, **lightning staff**.
- Armor: **holy robe** (Bless and Guard; `magic_defense` and `health_point` affixes). Potions: **blessing** (Bless) and **haste**.
- Affix: **initiative**.

### Fixed

- The races bat, dragon and kitten used the stat `magic_defence`, which does not exist, so their magic defense modifiers did nothing. They use `magic_defense` now. (`lib check` does not catch a misspelled stat in a modifier yet.)
- Descriptions that no longer matched the files (Guard, Entangle, Drain, Frostbrand, leather, plate, sword, priest, dragon, pup, ranger, healing potion).

### Known

- The loadout is not deduplicated: a class or race skill that a weapon or armor also offers is a second copy with its own cooldown.
- `bless` is offered by the holy robe and the blessing potion, and nothing else.
- Deep Freeze and Orc Rage stay retired (`rage` replaces Orc Rage for new characters).

## [2.2.0] - 2026-10-21

### Added

- Race **dragon** (Firestorm, a name generator with epithets, high hp and magic defense, slow).
- Affixes **health_point**, **magic_attack** and **physical_attack**.

### Changed

- The sword rolls a `physical_attack` affix instead of a fixed +3.
- Races have modifiers: bat (hp ×0.8, +2 initiative, +3 physical attack, penalties to magic), kitten (+1 initiative, +4 physical attack, weak to magic) and pup (hp ×1.3, −3 initiative, +2 defenses). The pup starts with Guard instead of Power Strike.
- Guard raises both defenses of the caster by 6 and of one random other ally by 3, for 2 rounds.
- The mage has 15% less hp (was 10%).

### Known

- Bat, dragon and kitten used the misspelled stat `magic_defence`, so their magic defense modifiers had no effect (fixed in 2.3.0).

## [2.1.0-rc4] - 2026-10-20

### Added

- Deep Freeze, back as a retired skill so characters that had it stay valid (it was dropped by accident in the previous release candidate).

## [2.1.0-rc3] - 2026-10-20

The split of attack and defense (PR #5).

### Changed

- `attack` and `defense` become **`physical_attack`, `magic_attack`, `physical_defense` and `magic_defense`** in `character.yaml` (10/10 and 7/7).
- Damage goes through **`mitigate(hit, target, kind, f)`** with the new root file **`k.yaml`** (physical 40, magic 40). Backstab, Flurry, Power Strike and Sweep use the physical kind and Frost Bolt the magic kind. Arcane Bolt, Chain Lightning, Drain and Firestorm deal true damage on magic attack.
- Guard, Orc Rage, Fog Potion, Bloodrage and the druid change both stats. Plate, the sword, Frostbrand, the staff and the mage use the new stat names.

### Added

- Affixes **physical_defense** and **magic_defense**. The old `defense` affix is retired (it points at `physical_defense`) so characters that rolled it stay valid. The robe offers `magic_defense`.

## [2.1.0-rc2] - 2026-10-20

Built on the 2.0.0 content, without the test items of rc1.

### Changed

- **Damage with diminishing returns**: `hit × 40 / (40 + defense × f)` instead of subtracting defense. Backstab, Flurry, Frost Bolt, Power Strike and Sweep use it. Base defense goes from 4 to 7.
- Chain Lightning hits enemies with a decreasing chance (100%, 90%, 70%, 40%). Firestorm has a 40% chance to thaw each frozen character and makes everyone carrying a frost item deal 2 less damage for 3 rounds (allies included). Frost Bolt can freeze (40%, or 20% to refresh) and deals half to a frozen target.
- Leather offers the `defense` affix instead of a random bonus.

### Added

- `slots.yaml` (how many affixes an item rolls, 1 to 6) and the `defense` affix.

## [2.1.0-rc1] - 2026-10-13

Test items for the engine mechanics (lifesteal, healing modifiers, effects, cleanse and immunity). This line was not carried into the later release candidates.

### Added

- Skills **Poison Sting** (dagger), **Deep Freeze** (Frostbrand), **Purify** (robe) and **Antidote** with an Antidote potion.
- Armors **Blessed Cloth** (healing received ×1.25) and **Antivenom Vest** (immune to poison). Poison tag page.

### Changed

- Drain steals life, the druid heals more, Poison Sting weakens its target.

## [2.0.0] - 2026-10-12

### Added

- Classes **druid** and **rogue**, races **bat**, **kitten** and **pup**, and the skill **Drain**.

## [1.1.0] - 2026-10-18

The version that runs in production.

### Changed

- The orc's Bloodrage condition has a tooltip ("health is below 50%").

## [1.0.0] - 2026-10-12

### Added

- The yaml-language-server schema line at the top of every blueprint.
- The player-facing text: `wiki/fights.md` and `wiki/stats.md` (with title and order) and the tag pages for fire and frost.

## Initial content - 2026-10-11

The first commit: classes mage and warrior, races human and orc, 14 skills (Arcane Bolt, Backstab, Chain Lightning, Firestorm, Flurry, Fog Potion, Frost Bolt, Guard, Healing Potion, Mend, Orc Rage, Power Strike, Second Wind, Sweep), the weapons dagger, Frostbrand, staff and sword, the armors ember robe, leather, plate and robe, and the potions fog and healing.
