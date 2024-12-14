
To clean up redirects (C++ redirects or engine redirects) run the resavepackages commandlet.

```cmd
"Drive:\YourEngineVersionOrSource\Engine\Binaries\Win64\UnrealEditor-Cmd.exe" "Drive:\YourProjectPath\YourProject.uproject" -run=resavepackages
```

To fix BP redirectors, you can either do it when right clicking on an asset or a folder OR add a flag to the commandlet.
