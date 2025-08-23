
When you go in the Plugins window in the editor and click on Package it will run a command similarly to this:
`"F:/UnrealEngine/EGL/UE_5.5/Engine/Build/BatchFiles/RunUAT.bat" BuildPlugin -Plugin="F:/UnrealEngine/Projects/Plugins/UnrealPrefabsDemo/Plugins/UnrealPrefabs/UnrealPrefabs.uplugin" -Package="F:/UEShortBuilds/UnrealPrefabs" -CreateSubFolder -nocompile -nocompileuat`

Format seems to be `<Path to RunUAT.bat> BuildPlugin -Plugin=<Path to.uplugin> -Package=<Path to packaging output> -CreateSubFolder -nocompile -nocompileuat`

Here is a .bat file you can run (use the vars to edit it)
```bat
set PATH_ENGINE="F:/UnrealEngine/EGL/UE_5.5/Engine/Build/BatchFiles/RunUAT.bat"
set PATH_PLUGIN="F:/UnrealEngine/Projects/Plugins/UnrealPrefabsDemo/Plugins/UnrealPrefabs/UnrealPrefabs.uplugin"
set PATH_OUTPUT="F:/UEShortBuilds/UnrealPrefabs"


%PATH_ENGINE% BuildPlugin -Plugin=%PATH_PLUGIN% -Package=%PATH_OUTPUT% -nocompile -nocompileuat
```
