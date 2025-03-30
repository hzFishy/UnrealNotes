# Core
- [Engine various structures](https://dev.epicgames.com/community/learning/paths/0w/beginplay)
- [The Unreal Engine Game Framework: From int main() to BeginPlay](https://www.youtube.com/watch?v=IaU2Hue-ApI)

# Classes and types
## Game Framework Objects
- Detailed list [here](https://wizardcell.com/unreal/persistent-data/#gameframework-objects) (WizardCell)
- Another more detailed list [here](https://github.com/staticJPL/Unreal-Engine-Core-Documentation/blob/2e08b368e292d2f27e856e5e6f23b21f40cdd76d/Unreal%20Engine%20Gameplay%20Architecture/Main.md) (JPL)
- [More](https://1danielcoelho.github.io/unreal-engine-basics-base-classes/) details on UObject, UClass, CDO and UBlueprint (See also [[UBlueprint]])

## Object
- `ClassPrivate` Is the `UClass` of the object instance. The thing you get back when you call `GetClass`.
- If a object was loaded from "a loader" (`FAsyncPackage` or `LoadPackageInternal`) it will have the `RF_WasLoaded` flag. This can be very useful to use on actors to know if they got loaded from disk or spawned at runtime. 
- [UObject Construction & Post Initalization](https://github.com/staticJPL/Unreal-Engine-Documentation/blob/6fb6beee4484bd4b15cf037dbccafc823e21457b/Unreal%20Engine%20UObject%20Construction%20%26%20Post%20Initalization/Main.md)
- [[UObject virtuals details]]

## Actors
- [Actor lifecycle](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-actor-lifecycle)
- Extra details in `Actor.h` file

## Blueprints
- [[UBlueprint]]
- [[Blueprint Saving]]

## Other types
- [FName and GameplayTag in depth - Baffled's blog post on the subject](https://itsbaffled.github.io/posts/UE/GameplayTags-And-FNames-In-Depth)

# Miscs
## Seamless and hard travel
- [Wizard Cell Persistent Data Compendium](https://wizardcell.com/unreal/persistent-data/#travel-seamless-vs-hard)
