
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

> [!Info] Config example to customize with results
> 
> [project config example](https://gist.github.com/hzFishy/5232c990d7a287ebb7c5fb1912efddba)
> 
> Results:<br>
> ![[Pasted image 20250210214916.png]]![[Pasted image 20250210215040.png]]
# When working each time

- start watch mode `npx rbxtsc -w` (should check if automatic)
- run `rojo serve` in a different terminal (because it locks execution inside)
