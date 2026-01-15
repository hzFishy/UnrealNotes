
# Engine & Editor

## Console window panel
You can have a cool panel with your console commands & variable in `Window->Console Variables`
![[Pasted image 20250411181357.png]]

## Saving console variables
You can either:
- use the console command window
- set the console variable value in the correct `.ini` in the correct section
- see `ConsoleVariables.ini` (special file that doesn't exist by default, can be copied from source engine).
- use a `UDeveloperSettingsBackedByCvars`
- or simply add the cvar under `[ConsoleVariables]` section in the correct `.ini` file

## List of existing commands and variables
### Miscs
- Object console commands list [here](https://dev.epicgames.com/community/learning/tutorials/dXl5/advanced-debugging-in-unreal-engine#objconsolecommand)
- `ShowDebug [Name]`, by default this will give details about the player pawn/character
- `ToggleDisplay` : disables all HUD
	- See also `Slate.GameLayer.ViewportSlotVisible 0` and `Slate.GameLayer.PlayerCanvasVisible 0`
- `show COLLISION` : displays collisions, works in PIE
- `slomo <new time dilation>`: changes the world time dilation
- `listtimers` show all active/paused/pending times.

### Rendering
See [[Debugging Graphics]]

### Physics
- See [[Console commands & debugging|Console commands & debugging]] (in `/Physics/`)

# Making your own console commands and variables

## Cheat Scripts
You can make a special console commands that can run multiple commands. See [docs](https://dev.epicgames.com/documentation/en-us/unreal-engine/animation-shortcuts-and-tips-unreal-engine#cheatscripts).

## `Exec`
- Check [how to use `Exec` meta attribute for supported classes](https://unreal-garden.com/docs/ufunction/#exec) and [how to add it to other objects](https://unreal.gg-labs.com/wiki-archives/common-pitfalls/exec-functions#how-do-i-get-other-classes-to-support-exec-functions)

## Statics
- For static variables (int32, float, bool, FString) see `FAutoConsoleVariableRef`
- For static commands see `FAutoConsoleObject` and the childs such as `FAutoConsoleCommandWithWorld` or `FAutoConsoleCommand` (I have some helpers for that in [[Unreal Engine/Extra/My plugins|My plugins]])

**Code snippets**

```c++
static float DrawingShowFlagsMaxDrawDistance = 3500;  
static FAutoConsoleVariableRef CVarDrawingShowFlagsMaxDrawDistance(  
    TEXT("BrutalPuzzle.Editor.DrawingShowFlagsMaxDrawDistance"),
    DrawingShowFlagsMaxDrawDistance,  
    TEXT("Limits the drawing of custom show flags to this distance"),  
    ECVF_Default  
);
```

```c++
void ClearTrackedPrefabReferenceComponents()  {}
FU_Console::FFUAutoConsoleCommand CClearTrackedPrefabReferenceComponents(
"Editor", "ClearTrackedPrefabReferenceComponents", "Clear all tracked components",
FConsoleCommandDelegate::CreateStatic(ClearTrackedPrefabReferenceComponents));
```


> [!Info]- Custom command declaration example
> 
> ```c++
> // example 1, inside a namespace
> static bool bDrawCableInGame = false;  
> void ToggleDrawCableInGame() { bDrawCableInGame = !bDrawCableInGame; };  
> BPG_Console::FBPGAutoConsoleCommand CDrawCableInGame(  
>    "DrawCableInGame", "Toggle to show debug data",  
>     FConsoleCommandDelegate::CreateStatic(ToggleDrawCableInGame), "CableManager"  
> );
> 
> // example 2 (Northstar)
> auto& ConsoleManager = IConsoleManager::Get();
> ConsoleManager.RegisterConsoleCommand(
> 	TEXT("CyGlass.ToggleOverlay"),
> 	TEXT("Toggle CyGlass overlay."),
> 	FConsoleCommandDelegate::CreateUObject(this, &UCyGlassExtensionSubsystem::ToggleCyGlassOverlay))
> );
> ```

> [!Warning]- About re-registration warning (console commands and vars)
> you might get the following warning `Console object named 'xxx' already exists but is being registered again, but we weren't expected it to be!`
> You can totally ignore it if it happens when you hot reload/live code. For more details read the comment above the UE_LOG line

# Run commands from code

## Blueprint
Use the `Execute Console Command` node, the player parameter is not required in all cases.
<br>![[Pasted image 20250411170027.png]]

## C++
We can use the same logic than the BP node (`UKismetSystemLibrary::ExecuteConsoleCommand`).
Internally it uses `ConsoleCommand` on the Player Controller if valid and `Exec` on the `GEngine` if not.
It seems safer to use the kismet function because it has some extra safety measures.

