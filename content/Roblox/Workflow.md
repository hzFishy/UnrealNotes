
- [Roblox Testing docs](https://create.roblox.com/docs/en-us/studio/testing-modes)

# Main tool manager
Download Aftman [here](https://github.com/LPGhatguy/aftman#installation)

# Rojo setup
- install Rojo Roblox plugin [here](https://rojo.space/docs/v7/getting-started/installation/)

**VSCODE extensions**
- Luau Language Server / luau-lsp
- Rojo - Roblox Studio Sync
- Stylua
- Selene (warning because this often overlaps with luau-lsp analysis)
- roblox-ui

# Roblox-ts setup
- https://roblox-ts.com/docs/setup-guide/
- https://roblox-ts.com/docs/quick-start

**VS CODE extension**
- roblox-ts
- ESLint

[Info about syncing with Rojo](https://roblox-ts.com/docs/guides/syncing-with-rojo)

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


**White spaces**<br>
White spaces are ignored when watch mode is looking for updates on file save.


**Folders**<br>
Once your root folders are configured on your filesystem, any subfolder made (if it has at least 1 script inside) will be replicated to Roblox Studio.


**Updating filesystem <-> Roblox files**<br>
You may be able to desync Roblox Studio files/folders with your filesystem, but once you resync everything will be added/moved/deleted to match your filesystem. **This only happens for files/folders in the `TS` folder**
In this example, everything in red will be removed on sync:<br>
![[Pasted image 20250211003429.png]]![[Pasted image 20250211003433.png]]
to avoid that see `$ignoreUnknownInstances` [here](https://rojo.space/docs/v7/project-format/#instance-description). use `aftman add UpliftGames/rojo@7.4.0-uplift.syncback.rc.20`


**What is inside `rbxts_include<br>`**
it stores runtime lib and the promise package, used by roblox-ts.


**Updating scripts in Roblox Studio script viewer**<br>
If you edit a file in Roblox Studio, nothing will happen on your file system, and it seems like Rojo wont try to update it again until you invoke a edit from a classic filesystem change.

It seems like a option will come for that <br>
![[Pasted image 20250210222919.png]]
To have it already check [this](https://github.com/UpliftGames/rojo/releases/tag/v7.4.0-uplift.syncback.rc.20) release fork of rojo.


**Referencing assets**<br>
- [Main page](https://roblox-ts.com/docs/guides/indexing-children/)
- [Automated with plugin](https://roblox-ts.com/docs/guides/indexing-children/#rbxts-object-to-tree-plugin-by-validark) (see also io-serve)

Example: <br>
![[Pasted image 20250211213528.png]]![[Pasted image 20250211213544.png]]
```ts
// access
let StarterGui = game.GetService("StarterGui");
StarterGui.LoadingScreenGui
```


**Auto imports for roblox services**<br>
use `npm install @rbxts/services`, then in VSCode if you start typing any roblox service by name it should suggests you to auto import.
