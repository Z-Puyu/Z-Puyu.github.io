---
title: "The Design of Complex Modular Combat Systems in Dungeon Slayer Project for CS3247"
date: 2025-04-17 20:00:00 +0800
categories: ["Portfolio Projects", "Game Design"]
tags: [projects, games, "game design", "unreal engine"]     # TAG names should always be lowercase
author: zpy
pin: true
math: true
---

## Overview

This post serves to explain the various design decisions and their rationales of the **combat systems** in *Dungeon Slayer*, my term project for the course CS3247 Game Development, in which I assumed the role of the main designer and implementer of many mechanics related to combat in the game.

> Disclaimer: due to the constraint in time frame, manpower and project scope, not everything included in the design has been actually implemented.
{: .prompt-tip }

For a presentation of the full project, please visit [the project's introduction post](https://z-puyu.github.io/posts/CS3247Project/).

## The Combat Mechanics in Dungeon Slayer

During the development of Dungeon Slayer, I divided the combat mechanics in the game into **two major phases**, each with specific questions to be answered by the design:

1. The **Pre-Combat** Phase, where the player *prepares the "instruments and tools"* to be used during combat:

    - What are the player's **abilities**?
    - How does each ability **contribute to the progress of a battle** or **change the state of a battle**?
    - How does the player **acquire a new ability**?
    - Can the effects of an ability be **changed**? If so, **what kinds of changes** are possible?
    - Are the abilities **permanently available** once obtained, or **dynamically acquired and removed**?
    - How do we work out the balance between **powerful abilities** and **weaker but essential abilities**?

2. The **In-Combat** Phase:

    - How is the **turn order** determined?
    - What determines/restricts the **player's actions** in combat?
    - **Does the enemies use the same mechanics as the player** to determine what they can do in their turn?
    - **How much information should the player have** regarding the enemies?
    - **How much information should the enemies have** regarding the battle's state? How do the enemies **use contextual information** to decide their next actions?
    - Do we encourage the player to **have abilities covering a wide range of effects**, or **specialise in a particular type of abilities**? How do we achieve that in design?

The aim of the design process of combat-related mechanics in Dungeon Slayer would then be to answer the above questions.

## Abilities for Combat

The first task here is to list down the things which a player can do to enemies, as these will determine **how a player can win a battle**.

This "list of things" is essentially the player abilities available in combat, or, in the case of our game which is a deck-builder, the "cards" to be used in combat.

### Modular Card Effects

In traditional card games like *Slay the Spire* or *Hearthstone*, a card on itself is the **most basic unit** of a combat ability, that is, a card's effects are **pre-defined** --- they can possibly be modified or "decorated" by other game items, but every card will come with a set of default effects by design.

I decided to not follow this approach mainly for three reasons:

1. Our project has a limited time frame, so it is not very realistic to handcraft a large number of cards and put them to test.
2. Our team lacks the expertise in game balancing, which implies that it would be extremely **difficult for us to balance out the numbers while ensuring that card synergies still work properly**, and this challenge will grow exponentially as we have more and more cards.
3. If we were to have a large number of cards, we also need to decide how each card can be acquired, which adds another layer of complexity as it means we need to add more contents (e.g., stories, map regions, loot containers or enemy types) to the game in order to **justify when and where a card should appear**.

Instead, I decided to further break down the concept of a card into smaller units: **card effects**. The motivation here is that in almost all card games, a large variety of cards is essentially built from a **relatively small set of effects which keep recurring in different variants**.

For example, two attack cards might only differ in their **hit probability, cost or damage magnitude**, but they both apply the same effect --- attacking.

Or, in some other scenarios, an attack can carry elemental effects (or enchantments as some games term it). No matter how many different enchantments there are, they all derive from the simple concept that you can **change the type of damage dealt by an attack**.

Therefore, to aim at **"creating variety in cards while keeping the scripting workload light"**, I designed the cards in a way that **only the effects are pre-defined in scripts**, whereas the process of **"making the cards"** is delegated to the player who would then **acquire card effects** and create their own cards from the available effects.

Let's say in our scenario, we have 7 basic types of effects:

- Attack
- Status effect
- Heal
- Defend
- Buff: improve a character stat temporarily
- Debuff: weaken an enemy stat temporarily
- Stun: force an enemy to skip its next turn

If we then define, say, **5 elemental enchantments for an attack or status effect** and **10 character stats that can be affected by buffs/debuffs**, we would have **35 different effects** (including a "physical damage" and a "bleeding effect" which do not associate to an elemental enchantment) which can be used to craft a card. By simple combinatorics, the number of distinct cards the player can create is **enormous**.

This also makes it easier to balance the numbers because every effect only has **one numerical property**, so it is easier to **assess the strength and value of an effect** as compared to a card with multiple effects and complex synergies.

In following this design, a side effect is that the player now has a **higher degree of creative freedom** in forming the deck. Now it's a good time to explain how the "making of a card" works in our game.

### Card Crafting

The card crafting system is inspired by **functional programming** and **object-oriented principles** in software development.

First, to suit the lore of our game, we shall call that "thing" which gets applied to an enemy when a card is played **"spell"**. We view the interior structure of every card as a **pipeline** consisting of **effect nodes**. The spell starts as an **empty object** and will traverse through the pipeline sequentially. Each effect node will contain a card effect (as described in the previous part) and acts like a **function** to *"compose"* that effect onto the current spell.

If you are familiar with languages with good support for functional programming, such as F#, this process looks like:

```f#
spell: Spell = empty |> effect1 |> effect2 |> effect3 |> ...
```

To support features like "multiple casting" to allow a card carrying multiple spells (e.g., a "fire attack" spell that heals the player at the same time), the pipeline was implemented using a tree data structure, and every spell is produced along a directed path from the root to a leaf.

Depending on how many spells an effect is composed to, a **cost multiplier** will be given to each effect node such that if an effect can apply to 10 spells on the same card, its cost will be significantly higher than the same effect applying to only one spell.

The crafting pipeline should also support **"encapsulation"**, that is, a **frequently used sequence of effects** can be saved as a **"macro effect"** so that cards can be manufactured more efficiently.

## Turn-Based Combat Mechanics

The design includes several common turn-based mechanics from other games, tweaked to suit the theme and lore of this project.

First, the player always takes the first turn to avoid the complication of re-computing the turn order. During the player's turn, the player will first replenish his or her hand by randomly drawing from the deck. Then, the player can *play a card*, *discard a card* or *finish the turn*.

In some turn-based games, the player's number of actions per turn is restricted purely by a fixed upper bound. I find this approach overly simplistic and fails to deliver a logical and fair combat experience which **takes into account the strength of the player's deck**. This is mainly due to the freedom in crafting cards with almost any effects, which means that the strength could **vary drastically** between different cards and between different play-throughs. Therefore, in our game, the player has a special stat **"Mana"** which is consumed when playing a card and re-fills for every new turn. This means that every action has a **weighted cost** according to **how much an impact it can cause to the combat state**, and as such the game can encourage more strategic card crafting and card usage overall.

While it is the player's turn, the player receives **partial information** about what the enemies will do in their next turn. For example, the player might see that an enemy is about to attack, but not the type and magnitude of the damage that attack can cause. This is a **deliberate design decision** to add in a bit of ambiguity and randomness to make the combat flow **less predictable**.

## Elemental Effects

I wish to make the elemental effects in our game more distinctive when compared against other fantasy-themed games where the elements are mostly a **"flag"** assigned to an attack to determine how much actual damage will be received based on the enemy's stats like elemental resistance, etc.

To make things more dynamic and interesting, I decided to **remove the concept of "elemental resistance" or "elemental weakness" completely** from the character stats, i.e., in our game there is nothing like "a skeleton warrior is naturally weak to light". Instead, elemental effects impact gameplay through what I call **"Marks"**.

Here's how it works: let's say I first play a card that carries **a fire enchantment**. When this card hits an enemy, instead of dealing extra elemental damage, it **adds a fire mark** to the enemy. The next step is interesting:

- if I play another card with **fire enchantment** to the same enemy **while it's marked with fire**, the card's effects are now **10% weaker** on that enemy;
- if I play a card with an **opposite enchantment** (**ice** in this case), the fire mark will be **removed** and that card becomes **10% more powerful** on that enemy;
- if I play a card with any other enchantment, **the fire mark stays** and **a second mark will be added** to the enemy corresponding to this enchantment.

As such, to activate an elemental effect to the player's own advantage, a card must be first played to **mark an enemy**, after which the player must choose a card with the **opposite enchantment** to **resolve that mark**. This effectively gives the player a very good reason to **have a wide range of cards with different enchantment effects** in the deck.

> Unfortunately, due to time constraint, this nice feature was not implemented in the current build of the game :(
{: .prompt-info }

## Enemy Actions: Designing a Responsive AI

Like the player, an enemy also has a series of abilities which it can use against the player in combat. The main challenges for enemy actions in combat are:

- We need the enemies to be **responsive**: their actions should be contextual and reflect the current state of the battle. For example, when the player is low on health, the enemies should see this opportunity and start to aggressively attack the player; when an enemy is marked with an element, it should realise the potential risks and start to use defensive abilities to increase its chance of survival.
- We need the enemies to be **less predictable**: if the enemy's actions are too deterministic, the player easily loses the fun in combat and surprising situations.
- We also need the enemies to **not be too unpredictable**: sounds contradictory, but if the enemies are way too chaotic, they would appear irrational and unrealistic. We still want the player to be able to observe some **general patterns** and use that to form some **heuristics** to their own advantage.

To achieve this, I designed and implemented a **utility AI framework**. This framework divides AI decision-making into 3 stages:

1. **Observe**: the AI will capture a "screenshot" containing **contextual knowledge** of the battle's state. For example, this includes the character stats of the player, the enemy itself and other enemies, the available abilities and possibly the player's deck (we can decide whether this should be open information to the enemies through play-testing).
2. **Think**: for each ability, the AI will **evaluate its utility score** using the contextual knowledge. The abilities are then ranked based on their utilities.
3. **Decide**: the AI will pick an ability to perform. Note that the AI does not necessarily perform the highest-utility ability. Depending on the specific algorithm used for ability selection, the AI might:

    - have some tolerance for **"good but not optimal"** actions;
    - deliberately select a less optimal action;
    - fluctuates the utility scores within a margin before making the final decision, or
    - focus on actions which are **better than at least half of all actions**, but **not differentiate among them at all**.

The selection algorithm would vary based on the type of enemies or some special conditions which occur during combat (for example, when an enemy is seriously injured it might "lose its mind" and start to take less optimal actions).

To add another layer to this, I also designed the utility score calculation to be **context-sensitive**.

First, it is aware of the **aggregate effect** of an ability. Let's say an enemy has two abilities: ability A deals a single powerful damage. Ability B deals a small amount of damage, but carries an elemental enchantment (yes, the player can be marked as well) and also applies some debuffs to the player. In this case, although the effects of ability B are "weaker" than ability A in terms of magnitude, they apply a wider range of effects which might be more advantageous in some scenarios. Therefore, I used the following formula for an ability with $N$ effects:

```txt
Ability Utility = Strongest Effect Utility + (1 - Strongest Effect Utility) × (Total Utility from Effects / N)
```

Here, every effect's utility is **normalised to** $[0, 1]$ before plugging into the formula.

Second, the enemy should be aware of the **character stats** before making a decision. For example, an enemy with low health should feel the urgency to heal itself; an enemy with full health should not consider healing at all; and if an attack can kill the player immediately, the enemy should definitely prioritise using that attack, etc. To achieve this, after the ability's utility score is computed, it will be weighted by a series of **considerations**. A consideration will **use a curve to describe how important an action is given the current state of the combat**. For example, a healing action with raw utility score 0.5 might be adjusted to 0.9 when the enemy's health is really low, and clamped to 0 when it's at full health.

There are some complexities in implementing the system and I have to admit that some parts of it were only implemented at a very fundamental level, but with these basic ideas, a talented programmer in the future might be able to revamp and enhance the implementation to a full-fledged utility AI controller.
