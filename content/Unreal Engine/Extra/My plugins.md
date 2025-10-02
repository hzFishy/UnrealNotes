## Unreal Prefabs ([Website](https://hzfishy.gitbook.io/unrealprefabs/))
The power of Child Actor Components and Unity Prefabs the Unreal way.
[Fab Link](https://www.fab.com/listings/dc2e38c1-8cf1-4143-8564-f5d9ac2be8d8)

## Common Perception System ([Github Repo](https://github.com/hzFishy/CommonPerceptionSystem))
A UE plugin that implements common AI perception systems.

## Common Interaction System ([Github Repo](https://github.com/hzFishy/CommonInteractionSystem))
UE5 plugin for common interactions between a source pawn and abstract objects.

## Common Inventory System ([Github Repo](https://github.com/hzFishy/CommonInventorySystem))
Basic implementation of an inventory system for UE5. Supporting default slots and a separate hotbar. Also adds a crafting system.

## Common Health System ([Github Repo](https://github.com/hzFishy/CommonHealthSystem))
A simple UE plugin that adds common features related to health systems.

## Fishy World Space Widgets ([Github Repo](https://github.com/hzFishy/FishyWorldSpaceWidgets))
A simple UE5 plugin adding more optimized world space widgets.

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
