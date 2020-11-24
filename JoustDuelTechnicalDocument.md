# Joust Duel

For our second year GPR5100 module, we are asked to make an online multyplayer game using the rollback technique. This game is being made on the Neko Engine using an already implemented rollback that we have to understand and use in order to make our game and networking work correctly. The goal is not to make a visually esthetic game or to create elaborate gameplay but rather to implement a very small, easy online multyplayer game using the rollback technique.

For this game, i didnt have to work much on the networking part myself, because my gameplay didn't use new networking packet or new functions regarding online functionality. Rather, i had to understand the way the engine worked and how i had to deal with the already done rollback techniques in order to create my Joust Duel gameplay.
For this reason, most of my wok was done in the four principal documents : rollback_Manager, player_Character, game_Manager and physics_manager.

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

When changing direction, players "flip" so they are always facing a specific direction. This is done by rotating the players.

Players are also constantly falling if they are not flying or already at the bottom of the screen. The movement speed is limited in each direction to a maximum to avoid having super sonic jouster.


### Player side respawning
The player have the possibility to keep moving in a direction without being stopped by the screen by respawning on the other side. They keep their vertical position aswell as their velocity when it happen. This is done in the physics_manager file by checking the position and velocity of each player body and moving them when they reach the side of the screen. A trick was to check the velocity of the bodies before respawning them in order to prevent an infinite case of respawn every time they were moved to the other side of the screen.

![](https://marvinschrd.github.io/Images/joust%20movement%20respawn.gif "Original Joust game for exemple")

insert respawn gif + respawn code

### Player/Player collision
The principe of Joust duel is to have two player run into each other for hits. For this reason it was necessary to detect player/player collisions.
The collision are handled in two different files : first in the physics_manager, then they are executed in the rollback_manager.
In the physics_manager, collision are detected by using the AABB of each bodies. When they collide, the entities are then given to the rollback_manager wich will decide what to do at this point.

insert aabb collision image

### Collision behavior and hit conditions
When the rollback_manager get the collision with its entities, he then has to deal with them. For the Joust gameplay, the player wich has the higher vertical position when colliding is the winner, so the other player lose a health point and players cannot hit each other with their back, they must face their target.
For that to happen, the ManageCollision function had to determine if the player that was hit, was in the right situation to lose health.
Two conditions are needed : the player is lower than the other and the other player is facing him.

![](https://marvinschrd.github.io/Images/joust%20hit.gif "Original Joust game for exemple")

At first, i implemented more conditions in order to allow back hits, wich mean that a player could run into its opponent back and hit him. It worked but, it also created a situation where the player could be hit by its opponent when trying to strike his back, at the moment the player x position became greater than its target and if the other player managed to go higher than him. It is in fact not an error, because the other player didn't hit him with his back but with is front, but it gave the feeling of being struck by your opponent back and for this reason, i finally decided to stay with a face to face only type of attack.

![](https://marvinschrd.github.io/Images/actual_collision_joustDuel.png "Original Joust game for exemple")

When actually hit, a player then loose a health point and goes into a state of invincibility wich prevent him from being hit again but also make him fall to the ground and block his movement.
