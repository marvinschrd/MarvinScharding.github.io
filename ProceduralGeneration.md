# Procedural generation for my Purge The Curse Game

## Context around this generation
This procedural generation was created for a school project during the GPR4400.2 module wich consisted of creating a game using a procedurally generated level and the use of artificial intelligence.

## About the game and what i had to do
Purge the curse is a 2d top down game where you play as a cursed adventurer who must survive and kill enemies in a procedurally generated dungeon in order to gather enough magic to break the curse and escape.

For this game, i had to generate a dungeon made out of multiple rooms that the player can visit and while i could have used a BSP(Binary Space Partitioning) technique to generate it, i came across this really cool and interesting blogpost where they used physics instead of an algorithm to start the generation of their level and rooms and i wanted to try using this approach.

## How i did it
### Generating rooms
I didn't want to procedurally generate each room every time because i wanted to be able to actually design them. That's why i used prefabs that i randomly select at the start of the generation.

For their positioning, i used the physic engine by adding a collider and a rigidBody and instantiating the rooms on top of each others. The engine then randomly move the rooms away from each other because of the rigidbodies and the colliders and prevent them from overlapping.

![](https://marvinschrd.github.io/Images/with2SpawnPositions2.gif)

### Detecting collisions between rooms
Since my procedural generation wasn't based on an algorithm using a grid and a tilemap, the rooms didn't knew who where their neghbours or where to connect to the other rooms. I used the collision between each of them to find a position inbetween the rooms that i would later use to connect them by opening the walls.
![](https://marvinschrd.github.io/Images/checkpositioncode.png)

