
# Engine & Editor commands

Object console commands list [here](https://dev.epicgames.com/community/learning/tutorials/dXl5/advanced-debugging-in-unreal-engine#objconsolecommand)
You can have a cool panel with your console commands & vars in `Window->Console Variables`
![[Pasted image 20250411181357.png]]
## Miscs Commands
- `ShowDebug [Name]`, by default this will give details about the player pawn/character
- `ToggleDisplay` : disables all HUD
- `show COLLISION` : displays collisions, works in PIE
- `slomo <new time dilation>`: changes the world time dilation

## Rendering commands
- `FreezeRendering`, good to use with `ToggleDebugCamera`, allows you to see how culling works, with extra stuff like seeing the POV of different steps of rendering (press B)
- `ToggleDebugCamera`
- `ProfileGPU` panel, also named "GPU Visualizer"
- `r.Streaming.PoolSize XXX`

## Physics commands
- See [[Unreal Engine/Physics/Debug|Debug]] (in `/Physics/`)

# Making your own console commands/vars
- [Details about `Exec` functions](https://unreal.gg-labs.com/wiki-archives/common-pitfalls/exec-functions)
- For static variables (int32, float, bool, FString) see `FAutoConsoleVariableRef`
- For commands see `FAutoConsoleObject` and the childs such as `FAutoConsoleCommandWithWorld` or `FAutoConsoleCommand`
- To save the cvars values, see `UDeveloperSettingsBackedByCvars`.

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
#if !NO_CVARS && TFC_WITH_CONSOLE  
    /**  
     *     
     *  @return "TFC.Debug.TheName"     
     *  @return "TFC.Debug.OptionalSubCatepgry.TheName"
     */
#define TFC_Console_Internal_BuildFullCommandString(SubNameString, SubCategoryString) \  
    FString::Printf(TEXT("TFC.Debug.%s"), *(SubCategoryString.IsEmpty() ? SubNameString : SubCategoryString + "." + SubNameString))  
  
    struct FTFCAutoConsoleCommandWithWorld : private FAutoConsoleCommandWithWorld  
    {  
       FTFCAutoConsoleCommandWithWorld(const FString& SubName, const FString& Help, const FConsoleCommandWithWorldDelegate& Delegate, const FString& OptionalSubCategory = "")  
          : FAutoConsoleCommandWithWorld(*TFC_Console_Internal_BuildFullCommandString(SubName, OptionalSubCategory), *Help, Delegate, ECVF_Default)  
       {}  
    };
//...
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

## Run commands from code

### Blueprint
Use the `Execute Console Command` node, the player parameter is not required in all cases.
<br>![[Pasted image 20250411170027.png]]

### C++
We can use the same logic than the BP node (`UKismetSystemLibrary::ExecuteConsoleCommand`).
Internally it uses `ConsoleCommand` on the Player Controller if valid and `Exec` on the `GEngine` if not.
It seems safer to use the kismet function because it has some extra safety measures.

