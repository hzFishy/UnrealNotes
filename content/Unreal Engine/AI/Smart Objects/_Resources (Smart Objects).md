
# Resources
- [Smart Objects Overview (Docs)](https://dev.epicgames.com/documentation/en-us/unreal-engine/smart-objects-in-unreal-engine---overview)
- [Smart Objects Quick Start](https://dev.epicgames.com/documentation/en-us/unreal-engine/smart-objects-in-unreal-engine---quick-start)
- [zomg's notes folder on Smart Objects](https://zomgmoz.tv/unreal/Smart-Objects/)

# Important classes
- `USmartObjectSubsystem`
	- You can get it from world using `World->GetSubsystem<USmartObjectSubsystem>()` or with the static method `USmartObjectSubsystem::GetCurrent(World)`
- `USmartObjectBlueprintFunctionLibrary`
