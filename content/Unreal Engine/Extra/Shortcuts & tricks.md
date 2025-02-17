# Shortcuts
All the keys can be changed in the editor settings

[Blueprint Editor Cheat Sheet](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprint-editor-cheat-sheet-in-unreal-engine)
## Viewport

### Camera movements
- Go up (E)
- Go down (Q)
- Change speed (Scroll Wheel)
### Edit selected object
- Move (W)
- Rotate (E)
- Scale (R)

### Miscs
- Focus `Move the editor camera pivot on the selected object` (F)
- Content Browser `Open the asset of the selected object (if any) in the latest actif content browser tab` (Ctrl + B)

## Blueprint
- Search `Nodes, keywords, variables, ...` (Ctrl + F)
- Rename (F2)
- Toggle breakpoint (F9)
- Variable (Get) `Place a GETTER of the dragged variable` (Hold Ctrl while dragging)
- Variable (Set) `Place a SETTER of the dragged variable` (Hold Alt while dragging)
- Branch `Place a Branch node` (B)
- Do Once `Place a Do Once node` (O)
- Gate `Place a Gate node` (G)
- Group `Place a Group encapsulating the selected nodes (if none selected it will have a default size)` (C)
- Sequence `Place a Sequence node` (S)
- For Each Loop `Place a For Each Loop node` (F)
- Delay `Place a Delay node` (D)


# Console commands
- `ToggleDisplay` : disables all HUD
- `show COLLISION` : displays collisions, works in PIE
- Object console commands [list](https://dev.epicgames.com/community/learning/tutorials/dXl5/advanced-debugging-in-unreal-engine#objconsolecommand)

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

## Other

**Automatically Break on BP Exceptions**
Add this in `DefaultEditorPerProjectUserSettings.ini`
```init
[/Script/UnrealEd.EditorExperimentalSettings]
bBreakOnExceptions=True
```

**Show BP Script Callstack on Warnings**
add this in `DefaultEngine.ini`
```ini
[Kismet]
ScriptStackOnWarnings=true
```



# Other
## Miscs

**Reset editor camera to before-PIE location & rotation**
> Disable `bEnableViewportCameraToUpdateFromPIV` in Editor settings

**Control Asset Open Location**
> Edit `AssetEditorOpenLocation` in Editor Settings

## Handle crashing for shipping builds

Enable `Include Crash Reporter` in the project settings
If you want to have a more detailed call stack, you need to include the debug symbols in your build, to do so enable `Include Debug Files`.

More info on how to use symbols to read mini dumps in shipping builds while not shipping the debug symbols directly to the user. [Time code](https://youtu.be/qT3E--_px28?si=vX0wjiT_cddlJyEC&t=624) (good for testing in a small environment)

**Custom crash reporter and receive crash logs** <br>
If you want to make the user send the crash report to you (and not epic games (by default)), you can use a external tool that supports UE crash reporter (some example are Sentry, Bugsplat and Backtrace) [Time code](https://youtu.be/qT3E--_px28?si=_WZ_iDdrTVkycQp2&t=1152).

