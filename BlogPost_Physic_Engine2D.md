# Technical blogpost about a 2D physic engine

## Context

This project was made for the GPR4400 course at the SAE Institute of Geneva Switzerland as the last project of the first year.
The goal was to understand the logic behind a 2D physic engine in C++ and to be able to implement every functions these kind of engine need in order to solve basic collision and physical reactions in a 2d environment.

## Ressources
To work on this project, i had to took inspiration and look for exemple, advices and explanations on a lot of different places.

My main inspiration was a physic engine from Nicolas SCHNEIDER, my teacher for this course at the SAE. [Links](https://github.com/Surue/SFGE/tree/GPR4400)
It helped me have a look at what a similar physic engine was made of and gave me inpiration for what i had to do.

I also used different internet pages and blogs to find informations, theory and implementations of parts of the physic engine.

[Links](https://box2d.org/documentation/index.html) to look at an entire and well known open source physic engine.

[Links](https://www.azurefromthetrenches.com/introductory-guide-to-aabb-tree-collision-detection/) theory about aabb tree collision detections
[Links](http://thatgamesguy.co.uk/cpp-game-dev-16/) Theory about quadtree

[Links](https://gamedevelopment.tutsplus.com/tutorials/how-to-create-a-custom-2d-physics-engine-the-basics-and-impulse-resolution--gamedev-6331) Article about basic collision and impulse

[Links](https://gamedevelopment.tutsplus.com/tutorials/collision-detection-using-the-separating-axis-theorem--gamedev-169) Article about the SAT theorem with images and exemples

[Links](https://developer.mozilla.org/fr/docs/Games/Techniques/2D_collision_detection) Arcticle about 2d collision detection that redirect to other good article about this subject

## What is the physic engine supposed to do

The physic engine that we had to implement was supposed to do and solve different things.

Solve movement and forces on different types of bodies : Dynamic, Static and Kinematic

Detect contacts and solve/update positions and velocities regarding the object shape and characteristics after each collision

Generate explosions and use raycasting

Execute different test to check the capacities of the engine

## My physic engine

Currently, my physic engine is not finished and cannot correctly detect contacts and solve collisions. However the basics are working and he is able to estimate possible collisions.

### Vector and matrix
To make this game engine, i first had to implement every basic classes that are then used for maths and physics and everywhere in the engine. The first ones where the vector classe and the matrix classe because i had to implement the different operators as weel as dot and cross product, magnitude and normalization of vectors that would later be used across the engine.

Exemples of operators :
![](https://marvinschrd.github.io/Images/exemple%20operator%20vector.png)     ![](https://marvinschrd.github.io/Images/exemple%20operator%20matrix.png) 


### AABB
Then, in order to start detecting possible collisons between objects, i implemented AABB (Axis aligned bounding boxes). These boxes are used to contain objects and their colliders in order to know when object might be colliding and to reduce the amount of contact checking between objects.

![](https://marvinschrd.github.io/Images/aabbOverlapSchema.png)

The AABB are made of two vectors "bottom left" and "top right" that can then be used to calculate their extent and center.
The main use of the AABB is to know if two of them are overlaping. This information allow us to know that the object they contain might be colliding too and to go into further contact checking, ignoring objects that are not overlaping.

![](https://marvinschrd.github.io/Images/gif%20aabb%20detection.gif)
![](https://marvinschrd.github.io/Images/overlap%20code.png)

### Collision detection
The next step is then to use SAT (Separating Axis Therorem) to actually know if two object that have overlaping AABB are colliding or not and to calculate, if it's true, the MTV (Minimum Translation Vector) in order to correct the position of object that would have had the time to penetrate inside of each other. 



This will be done by using a contact and contact manager class. The contactManager class is used to  further check if contacts are real by calculating the SAT regarding the different shape of colliders and to create them when they occure and the contact class is used to represent a contact between object and colliders and to solve them

This will then lead to the position and velocity corrections operations that make object realistically collide, bounce and move against each others. As said in the beginning, my engine is currently not able to make these corrections.


## Conclusion

As explained, the implementation of a 2d physic engine is long and need a lot of different skills from logic to math and physics and i still haven't finished mine.
I will work on what is needed, trying to finish the quadtree in order to greatly increase the optimisation of the collisions detection and most importantly, adding the actual collision resolution that make a physic engin what it is. I'am glad that i was able to do what i did regarding the lack of time i had on this project but more than what i actually did, i'm happy to have learnt that much and to have understood how a physic engine work. My problems came more from struggling to implement things than to understand what needed to be done.

 I will not leave this engine unfinished ( or at least not actually working) because i know how usefull this learning has been and his for me.




 
