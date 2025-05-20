# Core
- [Engine various structures](https://dev.epicgames.com/community/learning/paths/0w/beginplay)
- [The Unreal Engine Game Framework: From int main() to BeginPlay](https://www.youtube.com/watch?v=IaU2Hue-ApI)
- [[Garbage Collection]]
# Classes and types
## Game Framework Objects
- Detailed list [here](https://wizardcell.com/unreal/persistent-data/#gameframework-objects) (WizardCell)
- Another more detailed list [here](https://github.com/staticJPL/Unreal-Engine-Core-Documentation/blob/2e08b368e292d2f27e856e5e6f23b21f40cdd76d/Unreal%20Engine%20Gameplay%20Architecture/Main.md) (JPL)
- [More](https://1danielcoelho.github.io/unreal-engine-basics-base-classes/) details on UObject, UClass, CDO and UBlueprint (See also [[Unreal Engine/Engine & Editor/UBlueprint & UBlueprintGeneratedClass]])

## Object
- [[UObject]]
## Actors
- [[AActor]]

## Blueprints
- [[Unreal Engine/Engine & Editor/UBlueprint & UBlueprintGeneratedClass]]
- [[Blueprint Saving]]

## Other types
- [FName and GameplayTag in depth - Baffled's blog post on the subject](https://itsbaffled.github.io/posts/UE/GameplayTags-And-FNames-In-Depth)

# Miscs

## Core delegates
- See `FCoreDelegates`
## Seamless and hard travel
- [Wizard Cell Persistent Data Compendium](https://wizardcell.com/unreal/persistent-data/#travel-seamless-vs-hard)
## UWorld creation flow
- [link](https://ikrima.dev/ue4guide/engine-programming/uworld-creation-flow/)
## `GWorld`
`GWorld` (that shouldn't be used in gameplay) holds a pointer to the game editor world.
When in PIE it's the PIE world, and it switch's back to the editor world on PIE exit.
See `SetPlayInEditorWorld` and `RestoreEditorWorld`.

## Source call of Beginplay
Partial root is `AGameModeBase::StartPlay -> GameState::HandleBeginPlay - >GetWorldSettings()->NotifyBeginPlay() which finally sets World->SetBegunPlay(true);`

