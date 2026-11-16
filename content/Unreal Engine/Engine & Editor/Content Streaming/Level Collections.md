
This seems to be a way to optimize how levels are managed. This should be considered if you have many levels with many actors that aren't movable or interactive.

Stored in a `UWorld` as a `TArray<FLevelCollection>`.

When a level is streamed in `ULevelStreaming::SetLoadedLevel` will call `UWorld::FindOrAddCollectionForLevelStreaming`.

If `s.World.CreateStaticLevelCollection` is true (false by default) and `bIsStatic` is true for the  `ULevel` it will be added to a collection of type `ELevelCollectionType::StaticLevels` using `UWorld::FindOrAddCollectionByType`.