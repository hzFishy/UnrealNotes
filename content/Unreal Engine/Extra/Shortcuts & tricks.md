# Shortcuts
All the keys can be changed in the editor settings

[Blueprint Editor Cheat Sheet](https://dev.epicgames.com/documentation/en-us/unreal-engine/blueprint-editor-cheat-sheet-in-unreal-engine)

## Global
- Find & open any asset in a project (Ctrl + P)

## Viewport

### Camera movements
- Go up (E)
- Go down (Q)
- Change camera speed (Scroll Wheel)
- Move around focused object (Alt + mouse)

### Edit selected object
- Move (W)
- Rotate (E)
- Scale (R)
- Pilot (Ctrl + Shift + P)

## Faster Level Design
- Open level (Ctrl + O)
- Toggle Translucent selection (T)
- Group & Ungroup (Ctrl +G/Shift + G) [More details](https://www.unrealengine.com/fr/tech-blog/designer-s-guide-to-unreal-engine-keyboard-shortcuts)
- Focus `Move the editor camera pivot on the selected object` (F)
- Move selected object(s) without the 3 arrows: (X: Ctrl + Drag LMB), (Y: Ctrl + Drag RMB), (Z: Ctrl + Drag LMB+RMB)
- Select all similar (Shift + E) [More details](https://www.cbgamedev.com/blog/quick-dev-tip-54-ue4-ue5-select-all-of-the-same)
- Current folder feature, any asset dropped in the level from the CB will be placed in this folder (Select folder in Outliner -> Make current folder) [More details](https://forums.unrealengine.com/t/solved-normal-would-love-to-select-the-folder-that-actors-go-in-in-outliner/1168761/2?u=hzfishy)

## Other
- Content Browser `Open the asset of the selected object (if any) in the latest actif content browser tab` (Ctrl + B)
- Tab Navigation (Ctrl + Tab)
- Show navigation surface (P)
- Screen capture (F9)

## Blueprint
- Search `Nodes, keywords, variables, ...` (Ctrl + F)
- Rename (F2)
- Toggle breakpoint (F9)
- Variable (Get) `Place a GETTER of the dragged variable` (Hold Ctrl while dragging)
- Variable (Set) `Place a SETTER of the dragged variable` (Hold Alt while dragging)
- Branch `Place a Branch node` (B)
- Do Once `Place a Do Once node` (O)
- Gate `Place a Gate node` (G)
- Group `Place a Group encapsulating the selected nodes (if none selected it will have a default size)` (C)
- Sequence `Place a Sequence node` (S)
- For Each Loop `Place a For Each Loop node` (F)
- Delay `Place a Delay node` (D)


# Tricks

## Console commands
At the top of this page there are interesting console commands [[Console commands & variables]]
## Miscs

**Show 3D Widget on Vector variable**
> `meta=(MakeEditWidget)` in C++
> ![[Pasted image 20250412162241.png]]

**Keep Simulation Changes**
> [[Keep Simulation Changes]]

**Reset editor camera to before-PIE location & rotation**
> Disable `bEnableViewportCameraToUpdateFromPIV` in Editor settings

**Control Asset Open Location**
> Edit `AssetEditorOpenLocation` in Editor Settings
