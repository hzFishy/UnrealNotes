This speedrun is aimed to learn the basics of Unreal Engine with some blueprint & visual scripting basics. <br>
This page has a lot of content, you can easily navigate between categories using the Table of Contents on the right.

> [!Info]  About C++
> If you want to learn more about how to work with C++ in UE, I would recommend checking [Laura's C++ speedrun blog post](https://landelare.github.io/2023/01/07/cpp-speedrun.html).
# Access & getting started
Download Unreal Engine from the Epic Games launcher:
- Click on "Unreal Engine" on the left side bar
- On the top bar click on "Library"
- Add the UE version you want
- If needed, download extra content for your UE version by clicking on "Options" (after clicking on the small down arrow next to "Launch/Update/Install/Resume")

Once you launched a engine version you will be able to load a existing project or create a new one using a template.

> [!tip]
> You can directly open a existing UE project with the correct engine version from the "My Projects" area

Other UE stuff such as your assets from your Fab library are accessible below the engine version, if some are missing try to refresh using the refresh button.

# Interface anatomy
Here are the main components of the UE interface: 
## Global

![[Screenshot_31b.png]]


> [!example] Top bar (1)
> A lot of tools and access to more settings are from the drop down displayed there

> [!example] Tabs (2)
> Where all opened tabs are showed

> [!example] Details panel (3)
> When you select an actor or asset, its properties and settings appear here, allowing you to tweak and configure various parameters.

> [!example] Content browser (4)
> It serves as your primary tool for managing assets, such as meshes, textures, materials, and blueprints

## Viewport & Level
![[Screenshot_32.png]]
> [!example] Viewport (1)
> A view of the level

> [!example] Outliner (2)
> Displays a hierarchical list of all the actors in your current level, making it easy to select and organize them.
> > [!Tip]
> > You can make folders to better organize your actors

> [!example] External Top bar (3)
> ![[Screenshot_34.png]]
> - **(3A)** Mode: This panel contains various tools for editing your level, including landscape, foliage, and geometry tools.
> - **(3B)** Add actors.
> - **(3C)** Blueprint quick access.
> - **(3D)** Level Sequence.
> - **(3E)** Play control & settings.
> - **(3F)** Packaging options.
> - **(3G)** More settings.
> >[!tip] Scalability settings
> >
> >You can change the viewport scalability settings (also known as quality settings)<br>![[Pasted image 20241110090524.png|350]]
> ><br>If the settings are different from High, you can change them from the top left of the viewport.<br>![[Pasted image 20241110090649.png]]

> [!example] Inner top bar left (4)
> ![[Pasted image 20241107214259.png]]
> - Viewport settings
> - Viewport perspective
> - Viewport rendering method
> - Viewport displayed info

> [!example] Inner top bar right (5)
> ![[Pasted image 20241107214438.png]]
> <br>Left to right:
> - Edit mode (Select, Move, Rotate, Scale)
> - Pivot mode (Global/Local)
> - Surface snapping
> - Move snapping
> - Rotate snapping
> - Scale scapping
> - Camera Speed
> - Show more viewports

> [!example] World Settings (6)
> A lot of settings on the current world (~= Level)

## Blueprint
![[Screenshot_36.png]]
> [!example] Graph view (1)
> Here you can see and edit your blueprint code

> [!example] Viewport (2)
> Here you can see and move scene components inside the actor
![[Pasted image 20241107215302.png]]

> [!example] Components (3)
> Displays all components, you can edit or remove them or add some new ones.

> [!example]  Functions & Macros (4)
>A list of the functions and macros your blueprint class have

> [!example] Variables (5)
> A list of the variables your blueprint class have

# Engine anatomy & basics

## Introduction

### Conventions
Unreal Engine location, rotation and scale units are in centimeters. <br>
In UE, the forward axis is X and the up axis is Z.

### Coding
When you code in UE, you have two layers: the C++ layer and the Blueprint layer. <br>
C++ can be mandatory to use special features or tools. C++ is also a lot faster than Blueprint code, which can make it a very important choice depending on what logic you want to execute in your game.

The Blueprint layer is built on top of the C++ layer, either on already existing engine classes or on yours that you made in C++, which means you don't **need** to do anything in C++ and work on your game with only the Blueprint layer (not recommended for big or multiplayer projects).

A Blueprint is an asset that inherits from a class, for example `Actor` (`AActor` being the real C++ name). In a blueprint you can already see/edit what is exposed from C++ (for example variables or components).

### Actor blueprint hierarchy
Unlike other engines like Unity, you don't create a empty "Game Object" then add components. <br>In UE you create a new blueprint asset, and this blueprint can already hold logic.

> [!Warning]
> You can only create new blueprints from the content browser or using the Quick Add tool.
> This means that you can't make a **new** and unique blueprint classes/instances directly in a level, it must exist in the Content Browser.

You can extend any Actor class with `Actor Components`,  `Scene Components` or `Interfaces`.
- A `Actor Component` don't have any transform, it's just a block of whatever logic attached to your actor.
- A `Scene Component` is the same as an `Actor Component` but it has a transform and must be attached somewhere on your Actor (at the root of the actor or on any other `Scene Component`), it also support sockets.
- For `Interface` see [[Classes and inheritance#Interfaces]]

> [!Info]
> More about Actors in [[#Main classes]]

### Playing in editor
When you test your game in the editor (!= packaged build), you are in **PIE** (Play In Editor). <br>
You can find PIE settings here: <br>
![[Pasted image 20241110110229.png|350]]

## Structure
==TODO==
> Check the graph or watch the video about the basics of engine structure [here](https://dev.epicgames.com/community/learning/tutorials/98E/unreal-engine-begin-play-engine-structure?source=0w)

## Main classes

> [!error] For new programmers
> If you are new to programming or if you don't know what a `class`, `child` or `parent` is please read [[Classes and inheritance]] before going any further !

![[Pasted image 20241107215637.png]] 
*[Image source](https://dev.epicgames.com/community/learning/tutorials/ryZ7/unreal-engine-begin-play-blueprints?source=0w)*

> [!example] Actor
> "Actor is the base class for an Object that can be placed or spawned in a level. Actors may contain a collection of Actor Components, which can be used to control how actors move, how they are rendered, etc." - [UE Docs](https://dev.epicgames.com/documentation/en-us/unreal-engine/API/Runtime/Engine/GameFramework/AActor)
> > [!info] About persistence
> > Most actors (and objects) doesn't persist between levels (which usually means losing data).
> > There is a custom solution to make any actor transferred to the new target level, but the easiest way is to store the persistent data on classes that are already persistent (see persistent classes below).

> [!example] Pawn
> "Pawn is the base class of all actors that can be possessed by players or AI. They are the physical representations of players and creatures in a level." - [UE Docs](https://dev.epicgames.com/documentation/en-us/unreal-engine/API/Runtime/Engine/GameFramework/APawn)

> [!example] Character
>"Characters are Pawns that have a mesh, collision, and built-in movement logic. They are responsible for all physical interaction between the player or AI and the world, and also implement basic networking and input models." - [UE Docs](https://dev.epicgames.com/documentation/en-us/unreal-engine/API/Runtime/Engine/GameFramework/ACharacter)

> [!example] Player Controller
> "Player Controllers are used by human players to control Pawns." - [UE Docs](https://dev.epicgames.com/documentation/en-us/unreal-engine/API/Runtime/Engine/GameFramework/APlayerController)

> [!example] Player State
> "A Player State is created for every player. Player States should contain game relevant information about the player, such as score, etc." - [UE Docs](https://dev.epicgames.com/documentation/en-us/unreal-engine/API/Runtime/Engine/GameFramework/APlayerState)
>  > [!tip] *Persists between levels*

> [!example] Game Mode
> "Defines the game being played. It governs the game rules, scoring, what actors are allowed to exist in this game type, and who may enter the game." - [UE Docs](https://dev.epicgames.com/documentation/en-us/unreal-engine/API/Runtime/Engine/GameFramework/AGameModeBase)
> > [!tip] *Persists between levels*

> [!example] Game State
> "Is a class that manages the game's global state, and is spawned by the Game Mode." -  [UE Docs](https://dev.epicgames.com/documentation/en-us/unreal-engine/API/Runtime/Engine/GameFramework/AGameStateBase)

> [!example] Game Instance
> "A high-level manager object for an instance of the running game. Spawned at game creation and not destroyed until game instance is shut down." - [UE Docs](https://dev.epicgames.com/documentation/en-us/unreal-engine/API/Runtime/Engine/Engine/UGameInstance)
>  > [!tip] *Persists between levels*

> [!example] HUD
> "Base class of the heads-up display. This has a canvas and a debug canvas on which primitives can be drawn." - [UE Docs](https://dev.epicgames.com/documentation/en-us/unreal-engine/API/Runtime/Engine/GameFramework/AHUD) <br>
> The HUD class can be used as a isolated area to manage to create, show, hide and destroy UserWidgets.

> [!example] User Widget
> "UserWidgets are used in Epic Games' UI System, called **Unreal Motion Graphics** (UMG)." - [Cedric](https://cedric-neukirchen.net/docs/multiplayer-compendium/common-classes/userwidget/)<br>
> This is the class you will use to display text, menus, inputs and more.

> [!Info] To go further ...
> More info can be found on [Cedric's Multiplayer Compendium](https://cedric-neukirchen.net/docs/multiplayer-compendium/common-classes/) and the [Official UE C++ API Reference](https://dev.epicgames.com/documentation/en-us/unreal-engine/API/Classes)

## Blueprints
> A mix of my naming convention and what most people do: [[Working as a team in UE#For Designers]]

### Variables

Here is a list of the most used variables types.

| <div style="width:150px">Name & color</div> | <div style="width:300px">Description</div>                                                          | <div style="width:300px">Example of values</div> |
| ------------------------------------------- | --------------------------------------------------------------------------------------------------- | ------------------------------------------------ |
| ![[Pasted image 20241109130249.png\|100]]   | A boolean                                                                                           | `True` or `False`                                |
| ![[Pasted image 20241109130255.png\|100]]   | A integer number (equivalent to `int32`)                                                            | `-1`, `5`, `9521`                                |
| ![[Pasted image 20241109130301.png\|100]]   | A floating-point number                                                                             | `0`, `3,45`, `-0.1`                              |
| ![[Pasted image 20241109160626.png\|100]]   | A character container, useful to mute (edit) at runtime.                                            | `""`, `"Hello"`,`"Hello World!"`                 |
| ![[Pasted image 20241109160631.png\|100]]   | Same as a `String`, but not meant to be mutable. Mostly used for the localization system.           | English: `Hello`, French: `"Bonjour"`            |
| ![[Pasted image 20241109160635.png\|100]]   | A engine struct (`x`,`y`,`z`), mostly used for locations or directions.                             | `(0,0,0)`, `(3,-5,1)`                            |
| ![[Pasted image 20241109160639.png\|100]]   | A engine struct (`x`,`y`,`z`), used for rotations.                                                  | `(0,0,0)`, `(3,-5,1)`                            |
| ![[Pasted image 20241109160644.png\|100]]   | A engine struct, contains a Vector (for location), Rotator (for rotation) and a Vector (for scale). | `((100,50,0),(0,-90,0),(1,1,1))`                 |
| ![[Pasted image 20241109171409.png\|100]]   | A reference to an instance of a object.                                                             | `Null`, `Light`, `Actor`, `Camera`               |

> [!info] Reading and writing (`Get`/`Set`)
> When you use a variable, you either read its value (`Get`), or you write a new value (`Set`). <br>
> You can `Get` or `Set` a variable by typing `Get My Variable Name` or `Set My Variable Name` (You need to have the correct context to make it appear).
> 
> To place a `Get` node of your variable from the `My Blueprint` tab, hold `Ctrl` while dragging then release. To place a `Set` node hold `Alt` while dragging then release.

> [!info] Variable color
> Depending on the variable type, the pin and line color will be different. <br> ![[Pasted image 20241109171524.png|350]]

> [!Info]- More on the `Object` type
> The `Object` variable type has 4 "sub types". <br>
> ![[Pasted image 20241109170522.png]]
> - `Object Reference`: A reference to an instance of a object.
> - `Class Reference`: A reference to a class of a object.
> 
> The `soft` version is special, a soft reference (or pointer) is a type that contains a path to an asset (which can be none), and it contains a pointer to an instance of that asset (which can be null). <br>
> It is **very** important and common to use soft object/class references to load asynchronously assets (for example some sounds that doesn't need to be loaded at the beginning of the game).
> <br>It's also very useful if you want to get a reference to an actor placed in a level (this would use the actor absolute path in the level).
> - `Soft Object Reference`: A soft reference to an instance of a object.
> - `Soft Class Reference`: A soft reference to a class of a object.

> [!info]- More on the `Text` type
> When using a text variable, you can easily format it.
> For example, lets say we have an item struct, it holds the item name and price. We want to display in a shop dialogue text "`<Item name>` costs `<item price>`".
> 
> This is easily done using the `Format` node: <br>
> ![[Pasted image 20241110105200.png]]
> ![[Pasted image 20241110105151.png|550]]<br>
> Here the `result` will be `Apple costs 3`.
> **For each `{...}` in the `Format` input a new input pin will appear**

> [!Info]- `Enum` type
>Enums are used to give names to constants, which makes the code easier to read and maintain.
>Use enums when you have values that you know aren't going to change, like month days, days, colors, deck of cards, etc.
>
> I found a great comment on how to visualize a enum:
> > [!quote] A Reddit User
> > Think of an enum like a drop-down list, rather than just a blank text box you can type into. Using an enum ensures that only values you've predefined as part of that enum can be present in a particular field/variable, rather than using a raw string which could contain anything.
> 
> Example of a engine enum <br>![[Pasted image 20241109173407.png|350]]
> > [!tip] Custom enums
> > You can make you own custom enums.
> > - Create a `Enumeration` asset in the Content Browser <br> ![[Pasted image 20241109173619.png|200]]
> >- Add the values you want <br> ![[Pasted image 20241109173634.png|450]]
> >- Use them where you want <br> ![[Pasted image 20241109173708.png|450]]
 
> [!Info]- `Struct` type
> A struct is a container of other variables (also known as members/properties).
> > [!tip] Custom structs
> > 
> > You can make you own custom structs
> > - Create a `Structure` asset <br>![[Pasted image 20241109184701.png]]
> > - Add the members you want (you can set a default value for each of them) <br> ![[Pasted image 20241109185227.png|350]]
>
> > [!tip] Making/Breaking structs 
> > 
> > You can `Make` and `Break` any struct. This is very useful in some cases. <br>![[Pasted image 20241109185113.png|350]]
> > <br>You can break any struct pin by clicking on `Split Struct Pin` after right clicking on it. <br>
> > ![[Pasted image 20241109185821.png|250]] ![[Pasted image 20241109185715.png|250]]

> [!Info]- Containers (`Single`, `Array`, `Set` or `Map`)
> For almost every variables types, you can decide if your variable *container* type is `Single`, `Array`, `Set` or `Map`. <br>
> ![[Pasted image 20241109190356.png|200]] <br>
> By default it's `Single`, as the name implies, it contains only one value.
>
> **Array** <br> An array can be seen like a list. It can be empty, or contain multiple values.
> You can Get, Add, Remove, ... some of those actions require an index.
> ![[Pasted image 20241109190344.png|350]] 
> 
> 
> **Set** <br> A set is very similar to the array, but ...
> - ... all entries needs to be unique
> - ... there is no order in a set, it's like if you add a marble to a bag of marbles, you know what you added but you don't know how they got mixed up. So if you convert your set to an array, and try to get the value at index 0, you probably won't get the first element you previously added.
> 
> **Map**<br> A map is like a dictionary in other languages. A map contains a list of pair of keys and an associated value. To obtain a value you must therefore give the correct key, there is no order in a map.
> All keys must be unique.
> 
> On the left the key (Key) and on the right the associated value (Value). <br>
> ![[Pasted image 20241109190907.png]]<br>![[Pasted image 20241109190933.png]]
> 
> To get a value, you use the `Find` node <br>
> ![[Pasted image 20241109191114.png|350]]
> - Red square : The key to use
> - Green square: The returned value (if found)
> - Blue square: `True` if the value is found, `False` if the value wasn't found

> [!tip]- Promoting a pin to a variable
> You can also create variables by using **Promote to Variable**. <br>
> ![[Pasted image 20241109171952.png]]
> <br>By right-clicking the **New Light Color** pin and selecting **Promote to Variable**, we can assign a variable as the **New Light Color** value. <br>
> ![[Pasted image 20241109172007.png]]
> 
> *From [UE Docs](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprint-variables-in-unreal-engine?application_version=5.5)*

> [!tip]- Converting between different types
> It is possible to convert one type to another. This conversion occurs in a node.
> UE will sometimes do it automatically when you try to drag a pin to another, otherwise you just have to look in the context menu for the conversion between type `x` and `y`. The syntax is usually `x to y`.
> 
> For example, here, the `integer` type is converted to a `string`: <br>
> ![[Pasted image 20241109191622.png|150]]

> [!tip]- Variable options
> For each variable you declare in `My Blueprint` panel you can set extra options. <br>
> ![[Pasted image 20241109192006.png|400]]
> <br>More details on what each option does [here](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprint-variables-in-unreal-engine#variablesinthemyblueprinttab).

> [!Info] More
> You can find [here](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprint-variables-in-unreal-engine?application_version=5.5) more details about blueprint variables.


### Events, Functions & Macros

Sometimes you need a piece of code in multiple places. Instead of duplicating it, the best is to transform it into a event, function or macro. But what is the difference ?

Mainly:
- Events are used for thing that don't return anything and that can execute in parallel.
- Functions are used if you need output values.
- Macros are used if you need to use *time* nodes or if you want multiple exec output pins. 

| Type     | <div style="width:80px">Exec inputs</div> | <div style="width:100px">Variables inputs</div> | <div style="width:100px">Exec outputs</div> | <div style="width:100px">Variables outputs</div> | <div style="width:150px">Access</div>  | <div style="width:250px">Other</div>                                                                         |
| -------- | ----------------------------------------- | ----------------------------------------------- | ------------------------------------------- | ------------------------------------------------ | -------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| Event    | One                                       | One or multiple                                 | None                                        | None                                             | Can be called from anywhere if public. | The execution flow does not wait for the "end" of the event, it calls the event then continues in parallel   |
| Function | One                                       | One or multiple                                 | One                                         | One or multiple                                  | Can be called from anywhere if public. | Cannot contain any *time* nodes *(Delay, Cooldown, ...)*. <br>Can be overridden in parent classes if public. |
| Macro    | Multiple                                  | One or multiple                                 | One or multiple                             | One or multiple                                  | Can only be called inside the class.   |                                                                                                              |

> [!tip]- Overriding
> You can override a function in a parent class if the function is public and exposed.
> For example, the `Actor` has those function overridable by default: <br>
> ![[Pasted image 20241109193658.png|200]]
>
> > [!warning]
> > 
> > When you override a function, this means that the code in the parent(s) function(s) **won't** be called.
> > If you want to **add** code in your child class, but still execute code in your parent function implementation, add the `Super` node (can be called at any order).
> > ![[Pasted image 20241110110735.png|350]] <br>
> > ![[Pasted image 20241110110740.png|350]]

> [!tip]- Actor & Component delegates
> You can get events from exposed function by clicking on a actor or scene component and scrolling down in the details panel. <br>
> Here i selected my Box component and clicked on the `+` next to `On Component Begin Overlap`
> ![[Pasted image 20241109200944.png|300]] ![[Pasted image 20241109201057.png|600]]

> [!tip]- Collapsing nodes
> You can collapse code into a graph, function or macro to gain time. <br> ![[Pasted image 20241109193436.png|450]]
> 

### Context Menu

The context menu is what is shown when your right click in a BP graph.

**Filter** <br>
Depending on where you right click (and what you were dragging), the options are different, because UE filters out any actions for you that are "unrelated/don't make sense".

> [!tip]- Toggle filtering
> You can deactivate/reactivate this filter at the top right.
> 
> ![[Pasted image 20241108221533.png|250]]


**Keywords** <br>
UE saves you time, you don't need to type the full name of each node to get it. Just type in keywords.
> [!info] Example with basic math operations
> If you want to do a mathematical operation, you can type the symbol directly.
> 
> ![[Pasted image 20241108223556.png|250]]
> 
> This works with Add (`+`), Substract (`-`), Multiply (`*`), Divide (`/`) and more !

> [!tip]- Custom keywords
> Some keywords are premade (mostly for engine nodes), but you can add yours in the `Keywords` entry.
> <br>![[Pasted image 20241108223933.png]]<br>![[Pasted image 20241108223943.png]]<br>![[Pasted image 20241108223951.png]]


### Organizing nodes

There is different ways to keep your code organized. <br>
The most known one is **comments** ! In UE you can comment individual nodes, or comment inside a group.

**Individual node comment** <br>
Hover your node then click on the little bubble to show it (and do the same if you want to hide it).
<br>![[Pasted image 20241108225005.png|300]] ![[Pasted image 20241108224851.png|300]]

> [!tip]- Pinning a comment
> You can make a comment always displayed in a graph by pinning it, even when zoomed out.
> 
> ![[Pasted image 20241108225115.png]]
> ![[Pasted image 20241108225121.png]]


**Group comment** <br>
You can make a group by pressing the group key (`C` key by default), if you want to make a group around already placed nodes, drag an area with your mouse then press the group key.

![[Pasted image 20241108225351.png|400]]
 ![[Pasted image 20241108225405.png|400]]

> [!tip]- Custom Group Color
> You can change a group color **individually** in the details panel (with the group selected). There is also other settings.
> ![[Pasted image 20241108225753.png]]
> 
> If you want to edit the default comment color, you can change `Default Comment Node Title Color` in the engine settings
> ![[Pasted image 20241108225911.png]]

**Reroute nodes** <br>
Sometimes you will have lines that will overlap each other or with nodes. With a double click on the line you can create a `Reroute Node`. <br>
This is possible with execution and variables *lines*. Once created you can move reroute nodes wherever you want.

> [!info]- Example
> **Before:** <br>![[Pasted image 20241108230129.png|700]]
> <br>**After:** <br>![[Pasted image 20241108230143.png|700]]


**Categories**<br>
 In order to better organize your variables, functions and macros, you can create categories and an infinite number of subcategories. <br>
 ![[Pasted image 20241108230416.png]]

> [!info]
> Categories are also displayed in the context menu. <br> ![[Pasted image 20241108230458.png]]


### Debugging
Once you finished coding something, you will test it, and if it goes wrong (or if you want to be sure everything works as planned) you will **debug** your code.
<br>Debugging is the process of identifying and removing errors or unwanted behaviors.
Blueprint debugging features are disabled in packaged builds.

**Print** <br>
The most common way to debug is to `Print` something.
In UE, you can use the `Print String` or `Print Text` nodes.<br>
*Print Text is recommended if you want to print runtime values (from variables for example)*
![[Pasted image 20241110105613.png|500]]

> [!info] Print to Screen & Print to Log
> - **Screen:** Will show on the top left of your PIE window.
> - **Log:** Will show in the Output Log.

**Breakpoints**
> [!quote] From [UE Docs](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprint-debugging-example-in-unreal-engine)
> "Blueprint debugging is a powerful feature that provides the functionality to pause the execution of a game during Play In Editor (**PIE**) [...]. When debugging, you can step through any graph of a Blueprint or Level Blueprint through the use of Breakpoints."

> [!info]- Example
> For example, I placed a breakpoint on the cast node <br>
> ![[Pasted image 20241110102013.png|500]]
> <br>If I play and overlap with the box, my game will pause and the graph with the breakpoint will open
> <br>![[Pasted image 20241110102144.png|500]]
> <br>If you hover the pins you will be able to see what data is contained
> ![[Pasted image 20241110102257.png|650]]

> [!info]- How to place breakpoints
> All breakpoint options are available when you right click on a node. <br>
> ![[Pasted image 20241110101948.png|300]]
> <br>To quickly toggle a breakpoint on a node you can use the shortcut (`F9` by default).

> [!Info]- Execution control
> When a breakpoint is hit, your game is pause, which means you can decide what you want to do next.
> <br>![[Pasted image 20241110104414.png]] <br>
> More details [here](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprint-debugging-example-in-unreal-engine)

> [!warning]
> - Breakpoints placed on events won't be triggered.
> - You can only place breakpoints on impure nodes.

> [!Info] More
> More about the breakpoint features [here](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprint-debugging-example-in-unreal-engine)

**Watch** <br>
The watch feature works with breakpoints, it allows you to see a value of a pin **without hovering it manually**.

> [!info]- Example
> Here I am watching the value of `Delta Seconds` and my `Time` variable
> <br>![[Pasted image 20241110103726.png|600]]<br>
> When I press play and the breakpoint is hit, here is what i have:
> <br>![[Pasted image 20241110103806.png|600]] <br>
> After a few seconds, here is what I have:
> <br>![[Pasted image 20241110103834.png|600]]

> [!info]- How to start/stop watching a value
> To watch a pin you must right click on it and click on `Watch This Value`.
> <br>![[Pasted image 20241110103400.png]] <br>
> To stop watching a value, rich click on the pin and click on `Stop Watching This Value`. Or click on the lens (🔍) icon.

# Miscellaneous

## Viewport tools

> [!tip]- Fast duplicating
> If you select an object in the viewport, then hold `Alt` and drag a axis (location, rotation or scale) this will duplicate the selected object and let you move/rotate/scale the new one.
> > [!info] Example
> > <br>![[FastDuplicating.gif|400]]

> [!tip]- Modeling tool
> ==TODO==

# Training exercises
So now that you read all of that, you need some practice to be sure your understood everything AND to learn even more !

If you already have some projects ideas and the listed exercises seems to boring for you, don't hesitate to directly start to learn by doing your project. <br>**But be careful !** Don't overkill yourself with a very big game that will probably make you hate UE (because what you have to learn will be to big for a new UE user). <br>Start with something "small" or "medium", something that you like and can be scaled up when you will have more knowledge and motivation to make a great game !

> [!abstract] List of exercises
> ==TODO==

# More
More resources if you want to go further

> [!abstract] Useful links
> **In my notes**
> - [[Shortcuts & tricks]]
> 
> **Official**
> - [Beginplay - Engine breakdown](https://dev.epicgames.com/community/learning/paths/0w/unreal-engine-beginplay)
> - [Epic Games UE learn portal](https://dev.epicgames.com/community/unreal-engine/getting-started/games)
> - [Official UE C++ API Reference](https://dev.epicgames.com/documentation/en-us/unreal-engine/API/Classes)
> 
> **Not Official but great**
> - [Unreal Source Discord Server](https://discord.gg/unrealsource)
> - [Cedric's Multiplayer Network Compendium](https://cedric-neukirchen.net/docs/category/multiplayer-network-compendium/)


> [!abstract] Recommended YouTube channels
> - [Mathew Wadstein](https://www.youtube.com/@MathewWadsteinTutorials)
> - [Ryan Laley](https://www.youtube.com/@RyanLaley)
> > [!warning] YouTube warning
> > 
> > Sadly, a lot of UE YouTube content about programming (and mostly for BP) shows wrong/bad ways to program or use UE features. <br> If you want to keep using YouTube as your main learning tool (not recommended), please try to understand *why* a youtuber tells you something, and don't follow him like a sheep 🧠.

