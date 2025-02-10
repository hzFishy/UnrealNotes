
[Testing docs](https://create.roblox.com/docs/en-us/studio/testing-modes)


# Rojo setup
Download aftman [here](https://github.com/LPGhatguy/aftman#installation)

then install rojo Roblox plugin [here](https://rojo.space/docs/v7/getting-started/installation/)

VSCODE extensions:
- Luau Language Server / luau-lsp
- Rojo - Roblox Studio Sync
- Stylua
- Selene (warning because this often overlaps with luau-lsp analysis)
- roblox-ui

# Roblox-ts setup
https://roblox-ts.com/docs/setup-guide/
https://roblox-ts.com/docs/quick-start
VS CODE extension
- roblox-ts
- ESLint


# VS Code setup

You can exclude some files & folders (in Preferences->Settings->Workspace, search Files:Exclude), example: <br>
![[Pasted image 20250210214149.png]]

> [!Info]- Config example to customize with results
> 
> [project config example](https://gist.github.com/hzFishy/5232c990d7a287ebb7c5fb1912efddba)
> 
> Results:<br>
> ![[Pasted image 20250210214916.png]]![[Pasted image 20250210215040.png]]
# When working each time

- start watch mode `npx rbxtsc -w` (should check if automatic) (locks execution)
- run `rojo serve` in a different terminal (because it locks execution)

# Miscs info

**White spaces**
White spaces are ignored when watch mode is looking for updates on file save.

**Updating filesystem -> Roblox files after move**
After you initially "replicated" a TS script from the filesystem to Roblox Studio, if you move the file to another folder (didn't test if outside the main container, but this should never be the case), then update the file, Rojo will somehow update correctly the file in Roblox. 
Here I moved `LoadingScreen` script to a new subfolder in Roblox Studio, it updated but in the filesystem it didn't change: <br>
![[Pasted image 20250210222943.png]]![[Pasted image 20250210223002.png]]


**What is inside `rbxts_include`**
it stores runtime lib and the promise package, used by roblox-ts


**Updating scripts in Roblox Studio script viewer**
If you edit a file in Roblox Studio, nothing will happen on your file system, and it seems like rojo wont try to update it again until you invoke a edit from a classic filesystem change.
It seems like a option will come for that <br>
![[Pasted image 20250210222919.png]]