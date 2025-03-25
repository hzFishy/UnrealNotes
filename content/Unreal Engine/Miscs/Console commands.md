- `ToggleDisplay` : disables all HUD
- `show COLLISION` : displays collisions, works in PIE
- Object console commands [list](https://dev.epicgames.com/community/learning/tutorials/dXl5/advanced-debugging-in-unreal-engine#objconsolecommand)

> [!Info] Custom command declaration example
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


> [!Warning] About re-registration warning (console commands and vars)
> you might get the following warning `Console object named 'xxx' already exists but is being registered again, but we weren't expected it to be!`
> You can totally ignore it if it happens when you hot reload/live code. For more details read the comment above the UE_LOG line
