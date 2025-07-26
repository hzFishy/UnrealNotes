# Resources
- [Detailed Actor Replication Flow](https://dev.epicgames.com/documentation/en-us/unreal-engine/detailed-actor-replication-flow-in-unreal-engine)
- For a complete list of `DOREPLIFETIME_XXX` macros see [Property Replication Reference](https://dev.epicgames.com/documentation/en-us/unreal-engine/replicate-actor-properties-in-unreal-engine).
- See also [[Rep Helpers]]
- [[Custom replication override]] and [Conditional Replication](https://dev.epicgames.com/documentation/en-us/unreal-engine/replicate-actor-properties-in-unreal-engine#conditionalreplication)

# Flow
The majority of actor replication happens inside the [`UNetDriver::ServerReplicateActors`](https://dev.epicgames.com/documentation/en-us/unreal-engine/API/Runtime/Engine/Engine/UNetDriver/ServerReplicateActors) function. This is where the server first gathers all actors it has determined to be relevant for each client, then sends any properties that have changed since the last time each connected client was updated. 
The [`UActorChannel::ReplicateActor`](https://dev.epicgames.com/documentation/en-us/unreal-engine/API/Runtime/Engine/Engine/UActorChannel/ReplicateActor) function then handles the details of actor replication to a specific channel.

## General Property Replication Process

> [!Warning] Disclaimer
> A lof of things happen in the replication process, im not talking about everything, I can sometimes skip a lof of processing or function calls.

### `UNetDriver::ServerReplicateActors`
See [[NetDriver]].

In `UNetDriver::ServerReplicateActors` we iterate "all" client connections.

> [!Info] About "all"
> Not all connection are always iterated in one tick, see `NumClientsToTick`.

Next, we continue if the connection has a valid `ViewTarget`.
All of of stuff is done afterwards, but most importantly `UNetDriver::ServerReplicateActors_PrioritizeActors` is called which returns 2 priority sorted list (`PriorityList` and `PriorityActors` of type `FActorPriority`).

Finally we actually do stuff with the actor we can replicate this tick with `UNetDriver::ServerReplicateActors_ProcessPrioritizedActorsRange` which calls `UActorChannel::ReplicateActor`.
Then we call `UNetDriver::ServerReplicateActors_MarkRelevantActors` which are marked to be considered for next frame.

### `UActorChannel::ReplicateActor`
See [[Channel]], [[FObjectReplicator]] & [[FRepLayout & FRepState]]

This eventually calls `FObjectReplicator::ReplicateProperties` which is a wrapper of `FObjectReplicator::ReplicateProperties_r`. 
This function replicates properties to the Bunch (`FOutBunch`, which is a bunch used to be send). 

It will call `FRepLayout::ReplicateProperties` which "Writes out any changed properties for an Object into the given data buffer".
Then `FObjectReplicator::ReplicateCustomDeltaProperties` which calls `FObjectReplicator::SendCustomDeltaProperty` (Does delta serialization on dynamic properties).
This will eventually call `FRepLayout::SendCustomDeltaProperty`.

And starting here various path are followed depending on types, for example for a struct `ICppStructOps::NetDeltaSerialize` will be called.


## Handling new replicated objects

### Spawning replicated actor for client
The process seems to be the following:
`UNetConnection::ReceivedRawPacket` -> `UNetConnection::ReceivedPacket` -> `UNetConnection::DispatchPacket`.

I don't know how the Actor Channel exists/is created since the representing actor doesn't exist yet.
`UChannel::ReceivedRawBunch` (bunch is of type `FInBunch`) -> `UChannel::ReceivedNextBunch` -> `UChannel::ReceivedSequencedBunch` -> `UActorChannel::ReceivedBunch` -> `UActorChannel::ProcessBunch`.
Then we simply check if the representing actor of the channel exists, if not we call `UPackageMapClient::SerializeNewActor`.

### Remapping net GUIDs
When replicated objects are spawned on server, we have to spawn them on relevant clients to.
Since they use unique net GUIDs, we have to tell to the locally spawned objects what are their net GUID.
Thats the job of `UNetDriver::UpdateUnmappedObjects`, which will call `FObjectReplicator::UpdateUnmappedObjects`  which will trigger onreps on the newly created object.

## OnRep handling
The function responsible for that is `FObjectReplicator::CallRepNotifies` which calls `FRepLayout::CallRepNotifies` which uses `UObject::ProcessEvent` to run the OnRep function.

# Common Issues

## OnRep OldValue
When using OldValue in `OnRep_` functions, be sure to make your parameter a reference, otherwise your value will be broken.

Example:
```c++
// broken
UFUNCTION() void OnRep_HidingActor(TWeakObjectPtr<ATFCInGameCharacter> OldVal);
// works 
UFUNCTION() void OnRep_HidingActor(TWeakObjectPtr<ATFCInGameCharacter>& OldVal);`
```
