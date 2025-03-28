
# Engine & Editor commands

- Object console commands [list](https://dev.epicgames.com/community/learning/tutorials/dXl5/advanced-debugging-in-unreal-engine#objconsolecommand)
## Miscs Commands
- `ShowDebug [Name]`, by default this will give details about the player pawn/character
- `ToggleDisplay` : disables all HUD
- `show COLLISION` : displays collisions, works in PIE


## Rendering commands
- `FreezeRendering`, good to use with `ToggleDebugCamera`, allows you to see how culling works, with extra stuff like seeing the POV of different steps of rendering (press B)
- `ToggleDebugCamera`
- `ProfileGPU` panel, also named "GPU Visualizer"
- `r.Streaming.PoolSize XXX`


# Making your commands/vars
- [Details about `Exec` functions](https://unreal.gg-labs.com/wiki-archives/common-pitfalls/exec-functions)
- For static variables (int32, float, bool, FString) see `FAutoConsoleVariableRef`
- For commands see `FAutoConsoleObject` and the childs such as `FAutoConsoleCommandWithWorld` or `FAutoConsoleCommand`

Code snippet
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
> // example 1, works in constructor
> IConsoleManager::Get().RegisterConsoleCommand(
> 	TEXT("TFC.Debug.DisplayInGamePlayerCharacterWindow"),  
> 	TEXT("Show data for player"),  
> 	FConsoleCommandDelegate::CreateWeakLambda(this, [this]()  
> 	{
> 		bDisplayDebugWindow = !bDisplayDebugWindow;
> 	})
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

