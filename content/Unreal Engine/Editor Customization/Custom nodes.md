
# Resources
- [Guide for making a CustomThunk, UK2Node and FNodeHandlingFunctor](https://dev.epicgames.com/community/learning/tutorials/2lZj/unreal-engine-creating-a-uk2node-returning-a-reference)
- [Custom thunks TL;DR](https://gist.github.com/intaxwashere/e9b1f798427686b46beab2521d7efbcf#custom-thunks-tldr)

# Miscs

If you have 2 nodes for the same function you wanted to override with a custom k2node, you have to add `BlueprintInternalUseOnly=true` on your function

Params marked with `ExpandEnumAsExecs` are hidden through `FBlueprintEditorUtils::GetHiddenPinsForFunction`.

To make a custom Thunk + K2Node work with `ExpandEnumAsExecs` do the following (if its const):
- Declare what param is the enum with ExpandEnumAsExecs
- In your K2 class
	- set `bWantsEnumToExecExpansion` to true
	- override `IsNodePure` and return false.
