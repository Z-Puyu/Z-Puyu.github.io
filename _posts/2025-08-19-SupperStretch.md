---
title: "Project: Supper Stretch"
date: 2025-08-19 20:00:00 +0800
categories: ["Portfolio Projects", "Game Development"]
tags: [projects, games, unity]     # TAG names should always be lowercase
author: zpy
pin: true
---

> Disclaimer: The project code is simply a deliberate coincidence with a certain popular location in the university where the creator of this game is currently studying in, and it may or may not truly reflect the game's contents.
{: .prompt-tip }

## Overview

[This Project's GitHub Repo](https://github.com/Z-Puyu/Project-Supper-Stretch)

Project: Supper Stretch is a **self-initiated student game development project** whose aim is mainly to *practise implementing various commonly seen mechanics in role-playing video games*. **Due to the limited manpower, it is to be expected that the game is poorly balanced and feels incomplete in terms of level design and contents**.

Project: Supper Stretch is a third-person RPG featuring a dark-fantasy style. You play as an adventurer waking up in a cursed dungeon, finding no way out. To keep himself alive, you have to salvage anything useful in the dungeon, including equipments, valuables, and... anything that "seems to be" **edible**! Yes, Project: Supper Stretch elevates the dungeon adventure experience, especially in terms of realism, by completely discarding the idea of acquiring buffs through potions and amulets. Instead, you need to collect all sorts of bizarre and suspicious materials in the dungeon in order to **cook your food**! These can be plants grown in the dungeon, edible ingredients you found in the chests, or even fresh harvest from a hellish creature you have just killed.

Interestingly, the inspiration of Project: Supper Stretch came from a Japanese anime [ダンジョン飯 (Delicious in Dungeon)](https://en.wikipedia.org/wiki/Delicious_in_Dungeon) in which the author painted a rigorous ecosystem inside a dungeon to the extent that even among the monsters there is an extensive food web. I was fascinated by this worldview and realised that it just adds so much realism to what would be a conventional "slay-them-all" dungeon survival game experience. Hence, I decided to base my ideas on the concept of "cooking with dungeon creatures" to create the core experience of Project: Supper Stretch.

## Core Mechanics

### The Cooking System

Cooking is the essence of Project: Supper Stretch. In this game, you do not get the luxury of health potions that restore 20% of your health the moment you drink it. Instead, you have to restore health by eating like what you would expect an adventurer to do. The basic physical statistics of the player in the game consist of three attributes: **Health**, **Satiation** and **Toxin**.

- Health is nothing special as compared to any other game that has a health mechanic: when it drops to 0, you die and everything has to start over.
  
- Satiation is a player attribute introduced to highlight the cooking and eating concept. It starts at 100 and decreases slowly over time and there is nothing the player can do to pause it. This is pretty much like the real world where you have to get some food once after some time, so it is also crucial to realism and immersion. When Satiation gets too low, the player will feel hungry, which warns you that it is probably the time to eat. When Satiation drops to 0, you will start to **starve**: you health will gradually drop until you manage to eat some food.
  
- Toxin is introduced for two purposes. One, if all that the player does in the game is "play-eat-play" and so on, the gameplay soon becomes monotonous and repetitive, so I want to add some challenge to the action of eating itself. Two, it is to reflect the dungeon background, where the edible stuff we find probably should not be so clean. Every edible ingredient intrinsically comes with a Toxin value. If you have too much toxin intake, you will fall sick: in the game, this means that you get random damages to your health.
  

The simplest way to restore some Satiation and Health is to eat some edible **ingredients** - these can be parts harvested from dead monsters or things growing in the dungeon. However, eating raw food comes with its cost: you generally restore only a very small amount of Health and Satiation, but gains a significant amount of Toxin and other negative effects. That is where the **Cooking** workbench comes in. Inside the dungeon, you can find **Camp Fires**. These Camp Fires are holy fires which effectively cleanse the evil and filth in the dubious food you collect in the dungeon. At each Camp Fire, you have **12 hours** to rest and cook your food. To make this process more realistic rather than just a variant of some crafting system, there are two other types of items which can be put into your recipe: **spices** and **additives**. They generally do not provide Health or Satiation on their own, but give strong buffs to the effects of your cooked food. For example, by adding **salt** as an additive, you can achieve a **-20% Toxin increase** on the food you cook. Therefore, using appropriate spices and additives in your recipe is crucial for healthy meals.

### Dungeon Exploration

The game map in Project: Supper Stretch is an **infinitely extending dungeon with procedurally generated levels**. This is both a deliberate preference as I want to create a sense of unfathomable depths of the dungeon, as well as a compromise to the fact that I have little knowledge about level design.

### Combat

The combat in the game follows the traditional RPG components. The player has a weapon and a shield. The weapon has a combo mechanic which is implemented with **animation events** and a **combat controller** to coordinate the events. Using the shield, the player can gain some basic defence and block the enemies' attack to half their damage. The game also has a **parry** mechanic where if the player starts blocking just as the enemy starts to attack, the enemy will be knocked back with the attack completely nullified.

In terms of implementation, I created a **damage dealer** component to encapsulate a weapon and a **hit box** component to act as a damage detector. Both components have their own colliders and when the two colliders meet, the damage dealer will emit an event carrying the damage data comprising of:

- The damage's starting and hitting location
  
- The damage's magnitude
  
- The damage's initiator
  
The starting and hitting locations will be used to check if the damage is blocked. This is done by defining a **blockable range** as an angle inside the shield's script, and checking whether the signed angle between the damage incoming direction and the character's forward direction falls within this range.

## Changelog

See [the project's GitHub page](https://github.com/Z-Puyu/Project-Supper-Stretch/tree/main).
