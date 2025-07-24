# Resources
[Detailed Actor Replication Flow](https://dev.epicgames.com/documentation/en-us/unreal-engine/detailed-actor-replication-flow-in-unreal-engine)

# Flow
The majority of actor replication happens inside the [`UNetDriver::ServerReplicateActors`](https://dev.epicgames.com/documentation/en-us/unreal-engine/API/Runtime/Engine/Engine/UNetDriver/ServerReplicateActors) function. This is where the server first gathers all actors it has determined to be relevant for each client, then sends any properties that have changed since the last time each connected client was updated. The [`UActorChannel::ReplicateActor`](https://dev.epicgames.com/documentation/en-us/unreal-engine/API/Runtime/Engine/Engine/UActorChannel/ReplicateActor) function then handles the details of actor replication to a specific channel.

