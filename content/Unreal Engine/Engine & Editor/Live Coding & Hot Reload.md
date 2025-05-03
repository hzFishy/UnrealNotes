
Very good post about [Stop Live Coding: The broken promise of a Unity-like workflow in Unreal](https://dev.northstarhana.com/Unreal-Engine/Stop-Live-Coding).

To **completely remove Reinstancing & hot reload (not live coding)**
> https://dev.northstarhana.com/Unreal-Engine/Defeating-Hot-Reload-and-Reinstancing

## TLDR; What Can I do with LC?
*[imgur link](https://imgur.com/a/what-can-i-do-with-live-coding-UEoRFWs)* <br>
![](https://i.imgur.com/tZo2l7s.png)

## Overview of how Live Coding works
*(Thanks to Blue Man, UE Source Discord)*

It creates a new patch, which is actually an exe
It runtime links the exe with the running program and with information from the pdbs modified existing loaded dlls and exes and replaces the first instruction in each function with a jump instruction to the new function in the linked patch, so your code still goes to the old function but jumps at the first instruction to the new one 

And since a new patch is runtime linked into memory adding and changing static variables is actually fine, the new code will be reading from the new static variable from the new patch.

Generally most header changes are fully supported and work fine Adding new native classes and types works, adding new functions is fine, editing existing signatures is also fine.

What isn't fine is adding new reflected types and function.
Also adding new members isn't fine, regardless if it's reflected or not, same as adding virtual functions.
This is because the new code will expect a new memory layout, but if an object with the old layout ends up in the new code it will end in trouble.

Pretty much all runtime patching of native code follows this same exact process

in case of virtual functions is because the new code expects an additional entry in the vtable but it doesn't exists because it came from the old code.
