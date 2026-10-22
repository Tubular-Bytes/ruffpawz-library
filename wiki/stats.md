---
title: Stats
order: 20
---
Every character starts from the same base values: 1000 hp, 100 physical and magic attack, 70 physical and magic defense and 100 initiative. Class, race, weapon, armor and potion change them.

- **hp**: health. At 0 the character is out of the fight.
- **physical attack**: what physical skills are built on.
- **magic attack**: what spells are built on.
- **physical defense**: physical skills lose part of their damage to it.
- **magic defense**: spells that hit against it lose part of their damage to it. Some spells ignore both defenses.
- **initiative**: determines acting order in a round.

**How defense works.** Defense has diminishing returns. A hit keeps `400 / (400 + defense)` of its damage, so 70 defense takes about 15% off, 200 takes a third, and every extra point is worth a little less than the one before. Some skills let only part of the target's defense count, and a hit always does at least 1 damage. Damage that ignores defense (some spells, and damage over time) is not reduced at all.

