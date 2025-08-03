# Resources
- [[Networked Streamed levels]]
- [[_Resources (Level Design)]]
- [ikrima notes](https://ikrima.dev/ue4guide/networking/network-replication/sublevellevel-instance-streaming-replication/)
- See `UWorld` , `FStreamingLevelPrivateAccessor` and `FLevelUtils` 
# Level Streaming Volume
![[d.png]]

# Callstack on how world updates level streaming

- `UWorld::Tick`
	- `UWorld::InternalUpdateStreamingState`
		- `UWorld::ProcessLevelStreamingVolumes`

here it will do a bunch of stuff I don't fully understand for now

and at some point when a level is requested to be loaded/unloaded `APlayerController::ClientUpdateLevelStreamingStatus` will be called

When the streamed level state is set to `LoadedVisible` `ULevelStreaming::UpdateStreamingState` will call `UWorld::AddToWorld`