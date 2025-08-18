
- [Redirectors (Docs)](https://dev.epicgames.com/documentation/en-us/unreal-engine/asset-redirectors-in-unreal-engine)
- [Resave Assets (x157)](https://x157.github.io/UE5/Engine/Resave-Assets.html)


To clean up redirects (C++ redirects aka CoreRedirects or engine redirects) run the resavepackages commandlet.

```cmd
"Drive:\YourEngineVersionOrSource\Engine\Binaries\Win64\UnrealEditor-Cmd.exe" "Drive:\YourProjectPath\YourProject.uproject" -run=resavepackages
```

To make it faster you can also use `-NoShaderCompile -ProjectOnly`.
To fix asset redirectors you can add `-fixupredirects` or use the content browser asset action.
