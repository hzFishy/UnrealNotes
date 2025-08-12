
When you go in the Plugins window in the editor and click on Package it will run a command similarly to this:
`"F:/UnrealEngine/EGL/UE_5.5/Engine/Build/BatchFiles/RunUAT.bat" BuildPlugin -Plugin="F:/UnrealEngine/Projects/Plugins/UnrealPrefabsDemo/Plugins/UnrealPrefabs/UnrealPrefabs.uplugin" -Package="F:/UEShortBuilds/UnrealPrefabs -CreateSubFolder" -nocompile -nocompileuat`

Format seems to be `<Path to RunUAT.bat> BuildPlugin -Plugin=<Path to.uplugin> -Package=<Path to packaging output> -CreateSubFolder -nocompile -nocompileuat`

