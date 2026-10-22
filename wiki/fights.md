---
title: How a fight works
order: 10
---
A fight is played in rounds until only one team has anyone left standing. If it is still going after 100 rounds, it is a draw.

**Turn order.** In every round the characters act one after the other, the highest *initiative* first. Characters with the same initiative act in a random order.

**Skills.** A character has the skill of its class, the skill of its race, up to 3 skills picked from what its weapon and armor offer, and the skill of its potion.

**One skill per turn.** On its turn a character uses one skill that is ready. A character with nothing ready passes.

**Priorities.** Every skill in a character's loadout has a priority from 0 to 255, set by the player; 0 is the default. Among the skills that are ready, only those with the highest priority can be used, and if several share it, one of them is picked at random. A skill with a lower priority is passed over for as long as a higher one is ready. When priority decided the choice, the battle log says so, for example `uses guard (priority 3)`. If no priorities are set, every ready skill is equally likely.

**Cooldowns.** A skill with a cooldown of 3 is unavailable for the next 3 rounds after it was used. Every skill is ready at the start of the fight.

**Damage.** A skill works out its base damage, usually from the attacker's *physical attack* or *magic attack*. Modifiers the attacker has for damage it deals come next, then modifiers the target has for damage it takes. Damage never goes below zero, and a character at 0 *hp* is out of the fight. Healing cannot raise *hp* above the maximum.

**Effects.** Some skills leave an effect on a character for a few rounds: damage over time and healing over time work at the start of the character's turn, and a stun takes the turn away. A cleanse removes effects, and some effects can be removed by cleanses that name them (a freeze, for example).

**Modifiers.** Classes, races and gear change stats and damage. Within one value, all additions are applied first and all multipliers after that. Some modifiers come from skills and only last a few rounds.
