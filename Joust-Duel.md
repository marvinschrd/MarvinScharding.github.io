# Joust Duel

For our second year GPR5100 module, we are asked to make an online multyplayer game using the rollback technique. This game is being made on the Necko Engine using an already implemented rollback that we have to understand and use in order to make our game and networking work correctly. The goal is not to make a visually esthetic game or to create elaborate gameplay but rather to implement a very small, easy online multyplayer game using the rollback technique.

## Description and rules
Joust Duel is based ont the Atari 2600 game Joust and only focus on two player jousting against eachother. The players have to kill each other by flying and colliding. The player who has the highest altitude when colliding is the fight winner. Players can also drop an "Egg" wich kill the other player touching it when it falls. The first player to kill the other 4 times win the game.


![](https://marvinschrd.github.io/Images/joust.jpg "Original Joust game for exemple")


## Gameplay
Players can only move from left to right and will go up by flying in small burst. They will then slowly fall down when not going up.
Players respawn on the other side of the screen when they get offscreen on the right or on the left.
Players kill their opponent when hitting him with a higher altitude or by dropping an egg on top of him. They then get a point.
Players have a cooldown before being able to drop an egg.
First player to get 4 points win the game.

## Game level
The game level only consist on a platform at the bottom of the screen that allow players to land and move. The space on top of it is free in order to allow players to fly.

## Controls
Players can only move from left to right and will go up by flying in small burst. They will then slowly fall down when not going up.

"A" AND "D" KEYS : Move right to left

"SPACE" KEY : Fly up

"W" KEY : Dropping egg

## Challenges
The challenges will be to implement the game physics correctly :
- Players constantly falling when not going up
- Players moving from right to left
- Players being able to "fly" by small burst
- Collisions detection for player/player, player/platform, player/egg

And also to accordingly use the engine with its rollback implementation to make the networking functional :
- f






 
