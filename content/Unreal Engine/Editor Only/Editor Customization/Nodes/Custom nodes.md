
# Resources
- [Guide for making a CustomThunk, UK2Node and FNodeHandlingFunctor](https://dev.epicgames.com/community/learning/tutorials/2lZj/unreal-engine-creating-a-uk2node-returning-a-reference)
- [Custom thunks TL;DR](https://gist.github.com/intaxwashere/e9b1f798427686b46beab2521d7efbcf#custom-thunks-tldr)
- [Ramius intro tutorial](https://www.gamedev.net/tutorials/programming/engines-and-middleware/improving-ue4-blueprint-usability-with-custom-nodes-r5694/)
- [Ramius K2 nodes For Each Loop examples](https://github.com/MagForceSeven/UE-K2-Nodes/tree/main)

# Debugging

**Display Unique Names for Blueprint Nodes**
> See `Display Unique Names for Blueprint Nodes` in Editor Settings
![[Pasted image 20250717110508.png]]

**Get Kismet Compiler Intermediate builds**
<br>![[Pasted image 20250729182519.png]]<br>
![[Pasted image 20250729182508.png]]

# Miscs

If you have 2 nodes for the same function you wanted to override with a custom k2node, you have to add `BlueprintInternalUseOnly=true` on your function

Params marked with `ExpandEnumAsExecs` are hidden through `FBlueprintEditorUtils::GetHiddenPinsForFunction`.

To make a custom Thunk + K2Node work with `ExpandEnumAsExecs` do the following (if its const):
- Declare what param is the enum with ExpandEnumAsExecs
- In your K2 class
	- set `bWantsEnumToExecExpansion` to true
	- override `IsNodePure` and return false.
