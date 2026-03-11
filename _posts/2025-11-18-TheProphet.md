---
title: "The Prophet: A 2D Rogue-lite Deck-Builder Game"
date: 2025-11-18 20:00:00 +0800
categories: ["Portfolio Projects", "Game Development"]
tags: [projects, games, "game maker"]     # TAG names should always be lowercase
author: zpy
pin: true
---

[Beta release](https://github.com/Z-Puyu/The-Prophet/releases/tag/v0.1.0-beta)

## Overview

*The Prophet* is a single-player rogue-lite card game. The player controls a scholar whose soul is trapped inside a cursed living labyrinth. The scholar explores the labyrinth to uncover ancient spells and relics and use them to forge powerful magic to fight evil creatures that try to hunt him down. While learning the ancient arcane arts, the scholar gains the Vision - a mysterious force that allows him to foresee the unknown and reincarnate from death. Using the Vision wisely, the scholar will master the arts of magic through an endless cycle of death and rebirth and eventually cleanse the evil from the labyrinth.

The game appeals to players who like rogue-like deck-builders and strategic turn-based combat.

### Objectives

The objective of The Prophet is to explore a dungeon to collect cards and relics and use them to defeat the evil creatures in the dungeon.

### Progression

The player explores rooms in the dungeon and may engage in turn-based card combat with enemies. Different resources can be gained through exploration:

- Cards: used to fight enemies in combat
- Relics: used outside of combat to provide timed buffs
- Traits: gained through events or combat to provide passive abilities

### Deck-Building

The card combat revolves around elemental reactions. Dynamic elemental vulnerabilities and special card effects can be created in enemies by chaining up different cards, which are the key to gaining an advantage in combat.

### Rogue-lite Elements

The player dies when they lose a combat and will re-spawn in a new game run. Cards will be lost on death. Players may have gained up to 5 traits, and at most 3 can be kept for the next run.

## Core Mechanics

One unique mechanic of this game is its use of **incomplete information** in driving player decision-making. In a traditional deck-builder adventure game like *Slay the Spire*, players typically receive the full information about their opponents' next moves and the impacts of their opponents' actions before making a decision. However, in *The Prophet*, information is only **partially revealed** to the player.

For example, in battles, every card carries a Mark which defines the nature of the card’s power. Some cards will apply their Mark onto their target on top of their core effects. When a Mark is applied to a target, it may react with another Mark on the target to produce an additional effect. The Mark reactions are as follows:

- Fire: Reacts to Grass to add a Burn status effect to cause 1 damage per turn and reacts to Ice to turn it into Water.  
- Water: Reacts to Fire to execute extra damage.
- Grass: Reacts to Water to add a Poison status effect to cause 1 damage per turn.
- Lightening: Reacts to Water to trigger a Paralysed status effect to weaken the cards played by the target.
- Ice: Reacts to Water to trigger a Frozen status effect to restrict the number of playable cards for the next turn to 1 instead of 3.

An enemy could play a buff card on themselves, resulting in a new Mark added to the enemy itself. The player would thus want the next card they play onto the enemy to be able to react to the newly applied Mark. This means that by exploiting the cards played by the enemies, the player is able to create powerful synergies and combos.

However, at the same time every card played by the enemy has a "level of visibility". Higher levels of visibility expose more information about the card to the player. The player has a special resource called "Vision", which helps increase the visibility of the enemy's cards.  

Hence, it becomes very important for the player to manage their Vision as a core resource and utilise it to exploit the extra information of the enemy that is exposed.

During map exploration, the Vision also serves a special purpose in route planning. As opposed to *Slay the Spire* where the entire level's map is visible from the start, the rooms in *The Prophet*'s map are hidden by default. However, if the player manages to accumulate Vision, it increases the chance of revealing a room's contents before visiting it. This means that as a player progresses in the game, they need to constantly reevaluate their current strategy using the new information exposed to them, creating a unique dynamic decision-making experience.

### Vision Mechanics

Vision is a special player statistic and a core resource in the game used by players to plan their strategies and moves. It is used to foresee the contents in unexplored rooms and predict the cards played by the enemy during combat.

## Game Lore

The Prophet (you) is a medieval occultist scholar. He has just received a cryptic letter telling him to revisit the ancient caves that he used to practice at under the tutelage of his Master. His Master had vanished mysteriously one day, leaving him no choice but to learn and improve his magic skills on his own. Clinging on to the hope that he can reunite with his Master he decides to make the long journey back to his home ground...

This cave is actually a living creature made from the amalgamation of souls of past Masters. It turns out that your Master had been captured and assimilated into cave entity for the purpose of ensuring the longevity of the creature.

As you traverse the dungeons and defeat the enemies in your way, you come across clues (in the form of battered diary scrolls left behind by past Masters) that detail the gruesome nature of how they were slowly eaten away and used to sustain the cave entity.

Most importantly, the entries point to the existence of the First Master, who had a dangerous obsession with longevity and wanted to gain the elusive great power of ancient mages. After a failed attempt to cheat death through magic, he turned to the dark arts, and this living cave might have a deeper role to play in his grand plan of conquering all the magic powers of Prophets over the world to grant himself eternal life.

## Development and Personal Contribution

This game was developed as a student project for a game design course at Department of New Media and Communication, Faculty of Arts and Social Sciences in the National University of Singapore, in a **team of 5**. The project was developed with the **GameMaker** engine over a period of **9 weeks**.

In the project, I worked as the **lead game designer**, in charge of devising the main elements and mechanics of the gameplay, structuring and balancing game levels, adapting design decisions based on team discussions and play-test results, as well as maintaining the design documentation for the project.

I also contributed a considerable amount of effort to gameplay programming. I implemented the card logic and interactions, the turn-based combat loop as well as the buff mechanisms associated to the relic and trait mechanics of the game. I also worked on integrating sound effects and background music tracks into the game.
