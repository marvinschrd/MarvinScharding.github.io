# Joust Duel

For our second year GPR5100 module, we are asked to make an online multyplayer game using the rollback technique. This game is being made on the Neko Engine using an already implemented rollback that we have to understand and use in order to make our game and networking work correctly. The goal is not to make a visually esthetic game or to create elaborate gameplay but rather to implement a very small, easy online multyplayer game using the rollback technique.

For this game, i didnt have to work much on the networking part myself, because my gameplay didn't use new networking packet or new functions regarding the online part of the game. Rather, i had to understand the way the engine worked and how i had to deal with the already done rollback techniques in order to create my Joust Duel gameplay.
For this reason, most of my wok was done in the three principal documents : Rollback_Manager, Player_Character and Game_Manager.

## What i had to do

In order for the most basic Joust Duel gameplay to be possible, i had to recreate some of the original concept of the Joust game wich are the following in this case :
- Players horizontal and vertical movement (moving,flying and falling)
- Players side respawning
- Player/Player collision
- Conditions and behavior of hit

### Players movements

I implemented players movement in the player_character file. Players can use W,A,D keys and arrow keys dependinf on the player number.
Players can move left and right and they can fly up by pressing W or the UP key. The speed is not constant and is incremented in order to allow some kind of acceleration.

insert input image

Players are also constantly falling if they are not flying or already at the bottom of the screen. The movement speed is limited in each direction to a maximum to avoid having super sonic jouster.


### Player side respawning
The player have the possibility to keep moving in a direction without being stopped by the screen by respawning on the other side. They keep their vertical position aswell as their velocity when it happen. This is done in the physics_manager file by checking the position and velocity of each player body and moving them when they reach the side of the screen. A trick was to check the velocity of the bodies before respawning them in order to prevent an infinite case of respawn every time they were moved to the other side of the screen.

insert respawn gif + respawn code

### Player/Player collision
The principe of Joust duel is to have two player run into each other for hits. For this reason it was necessary to detect player/player collisions.
The collision are handled in two different files : first in the physics_manager, then they are executed in the rollback_manager.
In the physics_manager, collision are detected by using the AABB of each bodies. When they collide, the entities are then given to the rollback_manager wich will decide what to do at this point.
