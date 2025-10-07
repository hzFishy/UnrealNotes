
# Resources
- [Article](https://mintbanjo.com/articles/unreal-engine-5-spline-point-metadata/) on how to make a subclass of the spline component to have custom metadata for each spline point.
- You can also check the how the water plugin makes it owns subclass `https://github.com/EpicGames/UnrealEngine/blob/release/Engine/Plugins/Experimental/Water/Source/Editor/Private/WaterSplineMetadataDetails.cpp`

# Miscs
There isn't any built in events that fires when the spline is updated.
You can use `MarkRenderStateDirtyEvent` located at the `UActorComponent` level, which is called when the spline is updated.
