
# Fishy Utils ([Github](https://github.com/hzFishy/FishyUtils))

For now my only plugin, holds a lot of utility functions.
Most of them are in c++ namespaces, I will eventually make Blueprint Function Libraries for everything so BP users can use this plugin. 

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
- UI
	- `SetInputModeAndMouseVisibility`
- Miscs
	- Oriented Box
	- Socket picker

**Editor only features**
- Level Design features
	- **SelectSameFolderLevel:** In `Select` menu, or with the shortcut (`Shift+Alt+S` by default): Selects all other actors that are in the same root folder than the current selection.
- Message Dialog Box
