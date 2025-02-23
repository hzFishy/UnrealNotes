
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

# Engine & Editor commands
## Miscs Commands
- `ShowDebug [Name]`, by default this will give details about the player pawn/character


## Rendering commands
- `FreezeRendering`, good to use with `ToggleDebugCamera`, allows you to see how culling works, with extra stuff like seeing the POV of different steps of rendering (press B)
- `ToggleDebugCamera`
- `ProfileGPU` panel, also named "GPU Visualizer"
- `r.Streaming.PoolSize XXX`

