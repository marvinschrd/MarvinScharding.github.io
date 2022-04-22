
# Voliday : Creating dynamic User Interface

## Introduction :

### Context :
For my third and final year at SAE institute of Geneva in game programming, I had to work on a school project and create a city builder game with the other game programming students, the game art students and the audio students. This 8 month project is called Voliday and I had the role of being the UI/UX lead as well as a game programmer.
This blogpost is one of three blog posts about Voliday. You can learn more about the overall project in my Post-mortem blogpost about Voliday.

### Creating dynamic UI elements
As the UI/UX lead, and as it is furthermore explained in the Voliday post-mortem, I had to create and implement the various UI elements and menus of the game. As such,and since a city builder game need a lot of them, I tried to manually create as less Unreal “widgets” (in Unreal Engine, widgets are the name of the UI element that we can create and work with in the engine) as possible in order to not waste time and energy, as well as space in our project. 

Actually, much of the UI elements in this game were not fixed, and had to be updated at some point in order to display a change in information or value. Money counter, ressources informations, days and in game time… Still, some of them needed more logical optimization than others and these are the ones I will be talking about in this blogpost.

### Run time updating widgets :
The first UI elements I am going to talk about in this game are the elements that needed to be updated at runtime, depending on what the player was doing or what happened in game : The two types of buildings detail menus
images des menus 

These widgets had more than one or two elements that needed to be updated each time and most importantly, they had to be used for a lot of different in game objects.

Both of them are used in game to show informations about the buildings to the player and they have to display different values and other details for each of the different buildings present in the game. Since then, there was two possibilities for me :
Create widgets with the according informations and value for each of the different buildings. This means recreating a new widget each time a new building is added to the game or modify it each time a value or information is changed
Creating a single, or a few, base widgets that will be updated at runtime, and automatically with in game data. This means more implementation work but way less widget to create.

Obviously, I went with the second option.

In order to do that, we had to have all the informations for each building saved somewhere where it could be both easily modified by anyone, and easily accessible by the widgets when implementing their functionalities. This is why we created a global Building info spreadsheet, based on a specific struct which stored every information regarding buildings in a way that we called “building definition”. This spreadsheet made it possible for us to extract the base information of any building by giving it its type. This is what I used to update my widgets at runtime.

### Construction menu building detail widget :
The first widget I talked about, is the construction menu building detail. It is a small widget whose purpose is to show the most basic information about a building before a player chooses to place it in game. 

![](https://marvinschrd.github.io/VolidayPostMortem/images/DetailMenuBeforeAfter.png)

#### How it works :
The construction menu building detail is created once when the construction menu is initialized. He is empty, with only its design and layout but with no values in it. When the different building buttons are initialized in the construction menu, they store their associated building type. Then when the player is hovering the mouse on top of a building button, the UpdateValues function of the widget is called. This function uses the stored building type and the data spreadsheet to get all the informations of the building and set them inside the widget. The widget is used inside the built-in tooltip functionality of Unreal Engine to show and hide the widget when hovering the button. With this, only one construction menu building detail widget is created and present in game instead of one for each building. It simply updates itself every time we hover a building button.


#### Building detail menu widget :
This widget is the one that appears when a building on the map is selected. Its purpose is to show various basic information about the building as well as other varying information about the current status, generated money, customers.. of this building. It needs the same system as the previously talked widget with even more updates and runtime changes.

#### How it works :
The widget uses the same system of getting the building definition from the spreadsheet information to update its basic details but there is a catch : this time, the layout and the amount of information to show to the player are not the same depending on the building he selected. For it to work, I would have had to update the layout of the widget at runtime, but it is definitely not as simple and useful as only updating the values. Therefore, this time, I chose to create one widget of each type, each needed layout. It made me create 3 to 4 different models of this widget which is not optimal, but it was still way better than creating one for each building. 

Depending on the category of the selected building, and as explained on my other blogpost about the selection mechanics, the corresponding detail widget is called, updated and displayed on the screen.

![](https://marvinschrd.github.io/VolidayPostMortem/images/DwellingsDetailMenuBeforeAfter.png)


### The construction Menu :
The construction menu is the menu that displays all the buildings in the game based on their categories and is used by the player to buy and place buildings on the island. It is the most important menu in the game.

This widget contains a lot of other smaller widgets that work together to create a complete construction menu. This big widget doesn’t have to be updated multiple times at runtime, but it needs to be initialized when the game starts. This start initialization is done for the same purpose as the update functionality of the detail menus : to have a menu that doesn’t have to be manually changed and fixed everytime a new building is added in the game.

The construction menu initialize himself by doing a few things :

First, it has to get all the different building categories to know how many buildings are actually saved in each of them. The built-in function of Unreal engine to read spreadsheets doesn’t allow for column reading, but only rows . Since the categories are all stored on a column of the spreadsheet, I had to find a way to extract them and count the buildings in them. The construction categories widget loops through each row of the data spreadsheet and extracts the building definition of that row. Then, it takes the building category stored in these definitions and filters them. Filling an array with a specific building type each time the category is found. This ends up filling arrays for each different category  with the exact number of buildings of each category and their type (DWELLINGS : Inn and Castle, RESTAURATION : Fast food and Restaurant…)

![](https://marvinschrd.github.io/VolidayPostMortem/images/COnstructionMenuDuagram1.png)

Next, the category slide boxes use the size of each Building type array to determine how many construction buttons it has to create and add

Then, each category slide box initialize the buttons it created with the corresponding building type by looping through the category array filled with the buildings type

![](https://marvinschrd.github.io/VolidayPostMortem/images/ConstructionMenuDiagram2.png)

Finally, the building buttons themselves initialize their information based on the type that was given to them. The text, the image, and the customer icon as well as locking themselves if they are under a certain popularity level restriction.

![](https://marvinschrd.github.io/VolidayPostMortem/images/BuildingButtonWithText.png)

## Conclusion :
The final result is that with all of these steps, the most complex menu of the game can initialize itself and always be correct even if buildings are switched from a category to another in the game data, or if new buildings are added in the spreadsheet or modified in any way.

![](https://marvinschrd.github.io/VolidayPostMortem/images/ConstructionMenuGif.gif)

With this task, I learned and realized how important it is to implement dynamic UI elements when possible, in order to minimize the work that needs to be done globally, but specifically when things have to be changed. 

To go further, I could have tried to only have one single detail menu and also update the physical layout of the widget at runtime. I could also have pushed the generation of the construction menu by making it capable of adapting not only to new buildings being added in categories but also the adding of a brand new category.



