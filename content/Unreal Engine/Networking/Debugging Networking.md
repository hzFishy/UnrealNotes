**To debug networking side on rider**
watch `{,,UnrealEditor-Engine.dll}::GPlayInEditorContextString`

**Select a specific world**<br>
	![[Pasted image 20241028202522.png]]

# Logging categories
More details [here](https://dev.epicgames.com/documentation/en-us/unreal-engine/logging-for-networked-games-in-unreal-engine)

- `LogNetTraffic`
	- Is used when spawning replicated UObjects.
- `LogNetPackageMap`
	- About remapping NetGUIDs and serialization.

