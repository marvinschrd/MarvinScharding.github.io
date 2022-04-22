# Implementing Building interactions on a city builder
## Introduction :

### Context :
This blogpost is one of three blog posts about a third year school project from the SAE Institute of Geneva, a City builder game called Voliday. You can learn more about the overall project in my Post-mortem blogpost about Voliday.

Since Voliday is a city builder game, it means that the player has to build and place buildings in the game world but the interactions with them are also part of the gameplay. We had to implement different actions and mechanics regarding these interactions, and I did some of them which I will be talking about in this post.

For almost all the various click and selection building interactions, we created and used a specific manager called SelectionBehaviour which is a component attached to the UnrealEngine PlayerController. It manages most of the functionality I will be talking about.

## Highlighting buildings :
One of the first things I implemented about player and building interaction was the highlighting of the different buildings. The purpose of it is that I wanted the player to be able to immediately see which building he was looking at.

When a building is hovered by the player, a specific highlight outline can be seen around the building meshes. It makes it easier to see an entire building even if he is partially hidden by another.

![](https://marvinschrd.github.io/VolidayPostMortem/images/HighligthingBuilding.png)

### How it works :
I used the lineTraceByChannel function of the engine to trace a line from the mouse cursor into the world after converting the mouse location to worldspace.
When the trace hit something, I checked if the hit actor was a building by comparing its class. If the actor is a building, then he is registered in the CurrentlyHoveredBuilding and then the highlighting function is called.

The highlighting works by using a special highlight material which is used in a post process volume present in the game scene. Whenever a building has to be highlighted, the render custom depth of the building model is set to true and is automatically used by the post process volume with the highlight material to outline it. The previously highlighted building, if there is one, is deactivated so we only highlight one building at a time.

The highlighting material in itself is not something I will be able to explain here, and I followed instructions from this pretty good tutorial about highlighting models in unreal engine to create it. Most of the actual highlighting process that creates the building outline is done inside of the material.


## Selecting buildings :
We covered what happens when a building is hovered, before being selected, now we will talk about what happens when we actually click on them.

Selecting buildings in a city builder can have a lot of different uses. In our case, it is used to interact with the building and open its detail menu, giving the player access to other possibilities regarding the selected building such as viewing information about it or selling it.

### How it works :
Selecting buildings
When a building is selected, the mouse input is registered and, as explained previously, the CurrentlyMouseHovered building is then used to set the CurrentlySelected variable with the right actor. The selected actor also has to be highlighted in some ways for the player to be sure that it is the one that is currently selected. In order to do that, the materials of the building model are tinted with a specific color when selected and reset their initial textures when unselected.

## Opening detail menus
When a building is selected, the corresponding Detail Menu is opened and appears on screen. This widget, shown in my other post about the dynamic menus of Voliday, is the menu that allows the player to interact with the building. It’s the final step in selecting a building because this menu is the purpose of the selection. 

When a building is selected and registered, the SelectionBehavior calls the SelectBuilding function. This function does a lot of different things, some that I will not talk about here, focusing only on what I implemented myself. In SelectBuilding, the currently selected building actor is taken and I use its type (Inn, Market, Tavern..), extracted from its building definition, to open and update the correct detail menu,since the detail menus are not part of the buildings but updated at runtime. 

The five different models of detail menus are stored inside a specific widget called a widget switcher. I use this widget to easily switch between User Interfaces without having to deal with showing or hiding them every time when only one can be seen at once.

Depending on the selected building type, the widget switcher is called and switches to the appropriate model after updating its values with the building definition. For example, if the selected building’s type is a “Market” then the “Shops” detail menu will be updated and shown, but if the building's type was a “Castle”, then it is the “Dwellings” menu that would be called.

The player can select any building he wants and the switcher will automatically switch the menu, always displaying the information of the newly selected building. This is why the selection behavior component is so important. With it, and with the fact that it always knows what building is being looked at or selected, we can implement and use a lot of different selection features that will always be correct regardless of how many buildings are placed in game and how fast the player is switching from one to another.


![](https://marvinschrd.github.io/VolidayPostMortem/images/DetailMenuWhenSelected.png)


## Focusing the camera on the selected building
The last thing I implemented regarding building selection interaction is the camera focus. The camera focus was needed because when selecting a building, its menu would appear in the middle of the screen even if the selected building was not there but at another position. Repositioning the camera not only made the detail menus appear next to the selected building, but it was also useful in terms of user experience because the selected building would always be the center of attention. 

When a building is selected, the SelectionBehavior tells the camera to move to the building position. In order to do this, I implemented a function in the CameraBehavior component called MoveTo. This function takes in a 2D location vector on the X and Y axis and smoothly lerp the position of the camera, or more specifically the playerpawn where the camera is attached to, from its actual position to the selected building location.

The camera doesn’t zoom in or out but only horizontally moves to the given location. The cameraBehavior computes the distance between the camera and the selected building location at each frame and, using the game delta time, lerp their position together until the computed distance between the two is smaller than a fixed minimum value.





Lerping the position instead of just moving or teleporting the camera to a new location allows for a smoother movement, getting slower when the location is close to being reached.	

## Conclusion :
In conclusion, with the help of the SelectionBehavior, we were able to manage all the player/building interaction at the same place. This helped me add different features to the selection of buildings that can work with any type of building and be furthermore upgraded. Managing interactions with the in game actors is important from the basic gameplay point of view but also from the User experience point of view. Lots of the features we can think of are maybe unecessary in order for the game to work, but are small details that can greatly improve aspects of the player experience.

