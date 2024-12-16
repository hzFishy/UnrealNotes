# Core
- [Engine Components](https://dev.epicgames.com/community/learning/paths/0w/beginplay)
- [The Unreal Engine Game Framework: From int main() to BeginPlay](https://www.youtube.com/watch?v=IaU2Hue-ApI)

# Classes and types
## Object

- `ClassPrivate` Is the `UClass` of the object instance. The thing you get back when you call `GetClass`.
- If a object was loaded from "a loader" (`FAsyncPackage` or `LoadPackageInternal`) it will have the `RF_WasLoaded` flag. This can be very useful to use on actors to know if they got loaded from disk or spawned at runtime. 

## Actors
- [Actor lifecycle](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-actor-lifecycle)
- Extra details in `Actor.h` file

## Game Framework Objects
> Detailed list [here](https://wizardcell.com/unreal/persistent-data/#gameframework-objects)

## Other types
- [FName and GameplayTag in depth - Baffled's blog post on the subject](https://itsbaffled.github.io/posts/UE/GameplayTags-And-FNames-In-Depth)

# Miscs
## Seamless and hard travel
- [Wizard Cell Persistent Data Compendium](https://wizardcell.com/unreal/persistent-data/#travel-seamless-vs-hard)
