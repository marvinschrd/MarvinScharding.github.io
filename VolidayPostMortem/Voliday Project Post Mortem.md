# Post Mortem VOLIDAY

## Context :

For my third and final year at the SAE Institute of Geneva in game programming, I had the opportunity to take part in a specialization project whose goal was to make us work on a bigger project, with multiple other people and by “specializing” ourselves on specific tasks needed for the project. We also had to manage working with students from other fields such as game artists and sound and music producers.

### The project and initial constraints  :
The project was mandatory and had to be made from september 2021 to April 2022 for a duration of height months. We had some constraints, given by our stakeholders which were :
 to create a city builder game in the artistic direction of the Nintendo switch game  The Legend of Zelda, Links Awakening.
Make a game for windows PC platform
Use industry level tools
Collaborate with other school section to create the integrality of the game’s content by ourselves

To begin with this project, we started as a team of 8 game programmers, and we were later joined by the art team and the audio team, as well as external people which helped us to some extent. This made us a total of approximately 32 people across the 8 months of work we had to do.

Each of the 8 programmers were assigned or chose a specific role to specialize in for this project. Some of these roles were obvious and some others had to be given because of the type of project we had to do : it was the case of my main role, UI/UX lead.

## Project Voliday :

Voliday is the game we created. We chose to create a city builder on a volcanic island, where the player has to manage the island, build accomodations for tourists and use them and to gain popularity and money.

The game has been made on the Epic Games Unreal Engine 4, which was a discovery for us and a learning opportunity. It is playable on PC and entirely with a mouse or with mouse and keyboard

The programming team worked on the game, in the engine and implemented everything about the gameplay
The art team produced all the 2d and 3d assets of the games. From models to textures, images and icones.
The audio team produced the sounds effects of the game and the musics

### Voliday, how it went :
The voliday project was something very new for us because it was the first time we actually had to work with more than 1 or 2 other people or programmers. We also had to learn how to use the Unreal Engine and it took us a significant amount of time. We managed to work during the first three month but the game development was too slow and we made the mistake of not defining a specific and complete code architecture. Because of that, most of the progress we had made prior to january 2022 had to be refactored or done twice since it was very difficult for us to understand what had been done by everyone and how to continue this way. Still, we had some basis for the most important gameplay implementation and most importantly, we learned how to use the engine and how to keep working in a more organized and clean way.

After having refactored and defined a better code architecture, everything went better and faster and we managed to overcome most of our delay. The art team produced very good and very nice models and assets that we integrated in our game and the audio team did the same, which we integrated later. We had to cut a good amount of features that could not be added in time but we focused on what was already there and what had to be done for our goal product.



## My role : UI UX lead
As i said, my role was a role that was needed for this project because of the fact we had to make a city builder game. Since this type of games are heavily based on UI and UX interaction, we had to make sure it was something we were aware of and working on from the start to finish of the project. The UI in a city builder game is what makes the game playable, because the players are almost only using the various menus and interfaces of the game in order to play. The UX aspect is also a very important one, even though it is more difficult to apprehend and was more problematic for me.

### UI tasks:
My task was to imagine, design, and implement the various menus and user interfaces in the game. It is a task and a role I never had previously and was not really aware of but I took it as something much needed and an aspect I could learn a lot from too.

#### Imagining the game UI :
The first thing that i had to do, and that i did, was to define how many UI elements were going to be in the game. With the help of the project’s Game Design Lead, I defined every element that would have to be present in game. I used drawings and softwares to imagine the layout of the different menus and how to efficiently try to use them for gameplay and information to the player. It was a long and difficult task because even though we significantly reduced our ambition with the game during its production, the UI was still one of the most important and varied part of the project. This led to me making a lot of different mockups and getting feedback from them before trying to create them in the game engine. My task wasn’t to make the actual “visual” design of the UI, since this was made by the art team, but I had to imagine the layouts, and give them to the artists with instructions in order for them to create the 2d assets that would be used in the final game.

![](https://marvinschrd.github.io/VolidayPostMortem/images/AllUI.png)

![](https://marvinschrd.github.io/VolidayPostMortem/images/ConstructionMenuBeforeAfter.png)



#### Making the good choices :
The design of the user interfaces changed through the development, specifically after feedbacks, and playtest, and some of them were dropped because their related game content was no longer part of the project. Since the initial ambitions of our games were not the same as they ended up being, some of the work I did had to be adapted a lot of time. This is something we knew would happen but it can sometimes be painful. Another aspect was the fact that I had no experience in designing user interfaces, therefore, some of my ideas, quite a lot of them at first, weren’t as good as I thought they were. I also didn’t look as much as I should have into other city builder games for references and good practices. This could have saved us some time, especially on the UX part of the game, because we had to wait for actual experts or more experienced people to tell us what was wrong and had to be fixed or added.

Making things simple for the player was a hard task. In a city builder game, or any game that involves managing things, the amount of information and possibilities can quickly become overwhelming for the player if it’s not correctly displayed. Our game didn't have so many features for this to happen, but the UI had to work according to our game experience : simple, calm and peaceful. This is why I also took some of my first design inspiration on mobile games, because they often manage to display minimal information on a tiny screen without having the player wondering what he is doing or what is going on.


#### Implementing UI and gameplay at the same time :
An important challenge of my role was to then implement the UI in game. This was not always an easy task depending on the related gameplay mechanics but it was significantly harder than it could have been because of one good reason : the importance of implementing UI and gameplay at the same time. This is something I learned afterward because it is not how we, the game programmers, and I, worked during the first half of this project. When implementing a gameplay functionality, if some UI is related to it, it should be done at the same time. The mockups should be added in game and the gameplay then can be developed around it, even if it still looks really bad. This prevent what happened to me, realizing that the way a mechanic had been implemented didn’t suit the way it would have to be used by my designed user interfaces. I ended up having to significantly change or re implement some functionality just so it would work when linked to the UI. For example, the construction menu, which is a very important part of the game user interfaces, wasn’t implemented or added in game before the functionalities of building placement and I had to replace the various buttons previously in use and their functionality with the multiple categories and UI interactions used by the final construction menu.

#### UX thinking :
The UX aspect of my role is the one I mostly overlooked. Because it’s the one I was the most uncertain about. I immediately knew what I had to do or to try regarding the UI, but I didn't really know what were my UX responsibilities, how it worked with UI and where was the border between UX tasks and game design tasks which were done by the  game design lead. That’s why, even if my interfaces and implementation were progressing and working in game, some aspects related to “fun” or to great user experience were left behind by the majority of us in our work. We had to go back to user experience thinking later on, and focus ourselves on making our already functioning game a more pleasant experience.

This doesn’t mean I didn’t care at all about the user experience of the player while working, because I did, and i always tried to make everything as useful or pleasant as possible, but since I did not carefully detail some important aspect of our game experience, we missed some easy things that we had to work on later on.



## Conclusion :

I am very happy with what all of us, including the art and audio team, managed to do. Even if everything didn’t go as well as planned, we made the game into a reality and we are proud of this first experience on a project of this scale. The game corresponds to what we initially wanted to make and we are going to keep working on it, polishing it until the end of June 2022 at least in order to apply for the SAE awards.

Personally, even if the role of UI/UX Lead is not something I would have specifically chosen to take or to specialize in during a few months, I am satisfied with what I did and what I’ve learned from it. This task was needed and I know that a significant amount of the general success of our project has to come from it as well.



