# Resources
- See [[_Resources (Level Design)]]
- See [Docs Page](https://dev.epicgames.com/documentation/en-us/unreal-engine/world-partition-in-unreal-engine)

# World Partition
## Actors

### Configuration
![[Pasted image 20250803162047.png]]
> [Doc Section](https://dev.epicgames.com/documentation/en-us/unreal-engine/world-partition-in-unreal-engine?application_version=5.5#actors-in-world-partition)

**Grid:** You can force a grid assignment by replacing `None` by a grid name.
**Load control:** With `Is Spatially Loaded` and Data Layers you can manage when this actor can be loaded (See table).

| `Is Spatially Loaded` | Data Layer setup          | Actor loaded state                                                                |
| --------------------- | ------------------------- | --------------------------------------------------------------------------------- |
| Enabled               | Not inside any data layer | Will be loaded if streaming source close enough                                   |
| Enabled               | Inside a data layer       | Will be loaded if streaming source close enough AND if the data layer is enabled. |
| Disabled              | Not inside any data layer | Always loaded                                                                     |
| Disable               | Inside a data layer       | Will be loaded if the data layer is enabled.                                      |

### Editor management
In the World Partition Window, when you load a region it will call `ULevel::AddLoadedActors`. 
And when you unload it will call `ULevel::RemoveLoadedActors`.

**Delegates:**
- On add:
	- `OnLoadedActorAddedToLevelPreEvent`
	- `OnLoadedActorAddedToLevelEvent`
	- `OnLoadedActorAddedToLevelPostEvent`
- On remove
	- `OnLoadedActorRemovedFromLevelPreEvent`
	- `OnLoadedActorRemovedFromLevelEvent`
	- `OnLoadedActorRemovedFromLevelPostEvent`

## Streaming sources
Any actor can have a streaming source component (`UWorldPartitionStreamingSourceComponent`) and be enabled to load nearby cells.
The Player Controller is also by default a streaming source (it is using the `IWorldPartitionStreamingSourceProvider` interface, like the component).

## Grids
> [Doc Section](https://dev.epicgames.com/documentation/en-us/unreal-engine/world-partition-in-unreal-engine?application_version=5.5#runtime-grid-settings)

### Load methods
> [Doc Section](https://dev.epicgames.com/documentation/en-us/unreal-engine/world-partition-in-unreal-engine?application_version=5.5#loading-and-unloading-regions-in-the-editor)
- (Editor Only) By the World Partition Window
- (Editor Only) By location volumes
- By streaming sources

# Data Layers
> [Doc page](https://dev.epicgames.com/documentation/en-us/unreal-engine/world-partition---data-layers-in-unreal-engine)

- **Data Layer Asset:** Cross world data (made from Data Layers Outliner or Content Brower).
- **Data Layer Instance:** World specific (made from Data Layers Outliner).

## Debug Data Layers
> [Doc Section](https://dev.epicgames.com/documentation/en-us/unreal-engine/world-partition---data-layers-in-unreal-engine?application_version=5.5#debuggingandruntimeoverrides)


## Debug World Partition
> [Doc Section](https://dev.epicgames.com/documentation/en-us/unreal-engine/world-partition-in-unreal-engine?application_version=5.5#debugging-and-runtime-overrides)

# HLOD
> [Doc Page](https://dev.epicgames.com/documentation/en-us/unreal-engine/world-partition---hierarchical-level-of-detail-in-unreal-engine)

