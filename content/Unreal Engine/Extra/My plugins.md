## Unreal Prefabs ([Website](https://hzfishy.gitbook.io/unrealprefabs/))
The power of Child Actor Components and Unity Prefabs the Unreal way.
[Fab Link](https://www.fab.com/listings/dc2e38c1-8cf1-4143-8564-f5d9ac2be8d8)

## FishyWorldSpaceWidgets ([Github Repo](https://github.com/hzFishy/FishyWorldSpaceWidgets))
A simple UE5 plugin adding more optimized world space widgets.

## Common Interaction System ([Github Repo](https://github.com/hzFishy/CommonInteractionSystem))
UE5 plugin for common interactions between a source pawn and abstract objects.

## Fishy Utils ([Github Repo](https://github.com/hzFishy/FishyUtils))

Holds a lot of utility functions.
Most of them are in c++ namespaces, I will eventually make Blueprint Function Libraries for everything so BP users can use most of this plugin features. 

**Main features**
- General Utility functions
	- Runtime component add/remove
	- Compact prints (bool, float, vector, rotator, GameplayTag)
- Debug
	- Auto Console Commands
	- Draw Debugs
	- Modular logging macros (wrapper of [dbgLOG](https://github.com/itsBaffled/dbgLOG))
- Physics
	- `FindSkeletalOverlappingBodies`
	- ...
- UI
	- `SetInputModeAndMouseVisibility`
	- ...
- Miscs
	- Oriented Box
	- Socket picker
	- ...
- ...

**Editor only features**
- Level Design features
	- **SelectSameFolderLevel:** In `Select` menu, or with the shortcut (`Shift+Alt+S` by default): Selects all other actors that are in the same root folder than the current selection.
- Message Dialog Box
- ...
