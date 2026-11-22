# Overview

A realistic necromancy game coded in unity. It will be 3d as well as have a corpse-based system (need corpses to raise units). The units raised will be equivalent to what the npc were when they were living (procedurally generated).  
Necromancy will have various sub-systems for example: farming, trading and so on, to be performed by raised units. 

# Design

## Levelling

Levelling is separated into two sections:

* The players levelling system  
* The units levelling system

Levelling is restricted by a focus which for the player is necromancy and for units are chosen by the player. When a unit levels up they can pick a skill point from their focus only. The list of focuses are as follows:

* Necromancy  
* Healer  
* Warrior  
* Assassin  
* Elementist  
* Blacksmith  
* Enchanter  
* Alchemist  
* Tailor  
* Merchant  
* Farmer  
* Noble  
* Archer

The list of skill points for each focus will be decided later. To level up the unit will have to do actions relating to their focus. For example a warrior, assassin, elementist, archer will have to kill enemies in order to gain exp whilst a merchant will have to trade goods in order to gain exp.

### Player

The player will gain exp primarily through their units but can also gain exp from manually killing enemy units.  
The player will gain a third of the exp generated from any unit they have active.

As the player levels up they will gain the ability to have more focused units as well as automatic units. For every 10 levels they will gain one more focused unit as well as 20 more automatic units.

The player starts at 10 automatic units at level 1\.

#### Skills

* Unit Control: Allow the player to manually control focused units. Available at level 25  
* Swapping: Allows the ability to swap positions with a player and a focused unit. Available at level 50  
* Limit Breaker: Breaks the level cap of summoned units to allow them to level up with no restrictions. Available at player level 75  
* TODO Level 100 skill

### Unit

Units are further separated into two types in the context of levelling.

* Focused: Player has to choose the skill points and such  
* Automatic: Player chooses a certain focus and the game will determine the relevant skill points to choose

The reason for the above separation is as I am planning on the player having a lot of units, managing the levelling for each unit is too much effort for the player. Instead the player will be allowed to have a limited amount of focused units where they can develop their units as wanted (limit increases with player level) as well as automatic units who are managed by the game.

The level cap for all summoned units is 2 thirds of the level at the time of their death. The units can break through this limit by the necromancy gaining the skill Limit Breaker.

## Combat

Combat style is dependent on the weapon the unit has equipped (note for magic the combat style is restricted the focus)

* Unarmed  
* Sword (short sword, great sword, rapier, dagger)  
* Hammer (short hammer, great hammer)  
* Axe (short axe, great axe)  
* Bow  
* Magic (elementist, healer) \- restricted to certain focuses

## Necromancy

Automatic units can be controlled through different states:

* Follow  
* Passive  
* Stop  
* Defend

The automatic units can be told to follow either the player or the focused unit.

Focused units have the same states as the automatic units alongside the ability for the player to control them manually. This leaves the player vulnerable to attacks which can be mitigated by summoned units' defense state.  
This way the player can use sword, archer, assassin skills alongside having the summoned units follow them.

Each summoned unit will take a certain amount of mana as upkeep which is calculated through some calculation that takes into account the difference between the player level and summoned unit level. The player cannot summon units that are more than two time then their level.

### Corpse-based system

This is the main system where the player can summon units from existing corpses.  
The focus a unit has is dependent on the focus they had when they were alive. Their level as a result of dying has been reduced to two thirds of their original level. Their level cap has also been reduced to the level they were at the time of their death (can be bypassed through the player's skill Limit Breaker).

## Subsystems

Subsystems are systems that are supplementary to the game that can be accessed through a player's summoned units. The list of subsystems are:

* Trading  
* Crafting (Smithing, Alchemy, Enchanting, Farming)

## Story

1. Starting from a prisoner, player finds a dead necromancer in the cell and learns necromancy from their spirit  
2. The player escapes from the prison and arrives at another, where they use the cemetery to gain more units  
3. Their main goal from here is to get stronger so they can resurrect the necromancer  
4. They will do this by having 6 active "heroes" as their summons which they will sacrifice to revive the necromancer  
5. The player can then choose to resurrect the necromancer. Once resurrected the necromancer will escape the user and a new quest will be created to hunt down the necromancer as the necromancer will repeatedly raid the player with their units. Their units will consist of heroes from other kingdoms. If the raid is successful then the necromancer will steal one focused unit from the player and if not then the necromancer will escape.   
6. Once the necromancer is killed by player then the user will be able to resurrect the necromancers units  
7. Following the necromancer's death, heroes from other kingdoms will repeatedly come with their army trying to kill the player.

### Points

# Learning

This section is more for my benefit and is not directly related to the actual game design. More so notes on actually implementing. Will be separated into the vault when necessary

* Movement  
* Animation  
* Combat  
  * Summoning and controlling units  
  * Enemy units  
* Inventory  
* Levelling  
* Procedurally generating units (Corpse based system)  
* Subsystems (farming, trading and so on)  
* Story

## Movement

