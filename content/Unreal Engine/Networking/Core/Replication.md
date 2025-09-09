# Resources
- [Detailed Actor Replication Flow](https://dev.epicgames.com/documentation/en-us/unreal-engine/detailed-actor-replication-flow-in-unreal-engine)
- For a complete list of `DOREPLIFETIME_XXX` macros see [Property Replication Reference](https://dev.epicgames.com/documentation/en-us/unreal-engine/replicate-actor-properties-in-unreal-engine).
- See also [[Rep Helpers]]
- [[Custom replication override]] and [Conditional Replication](https://dev.epicgames.com/documentation/en-us/unreal-engine/replicate-actor-properties-in-unreal-engine#conditionalreplication)
- For interesting logging categories see [[Debugging Networking]]

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
See [[Channel]] & [[Net Types]].

This eventually calls `FObjectReplicator::ReplicateProperties` which is a wrapper of `FObjectReplicator::ReplicateProperties_r`. 
This function replicates properties to the Bunch (`FOutBunch`, which is a bunch used to be send). 

It will call `FRepLayout::ReplicateProperties` which "Writes out any changed properties for an Object into the given data buffer".
Then `FObjectReplicator::ReplicateCustomDeltaProperties` which calls `FObjectReplicator::SendCustomDeltaProperty` (Does delta serialization on dynamic properties).
This will eventually call `FRepLayout::SendCustomDeltaProperty`.

And starting here various path are followed depending on types, for example for a struct `ICppStructOps::NetDeltaSerialize` will be called.

## How are subobject's handled
When calling `AActor::AddReplicatedSubObject` it will eventually call `FReplicationSystemUtil::BeginReplicationForActorSubObject` then `UEngineReplicationBridge::StartReplicatingSubObject`.
This will run `UObjectReplicationBridge::StartReplicatingNetObject` & `UReplicationBridge::InternalAddSubObject` (which is called `FNetRefHandleManager::AddSubObject`).

## Handling new replicated objects

### Spawning replicated object for client
Inside `UActorChannel::ReceivedBunch` -> `UActorChannel::ProcessBunch` -> `UActorChannel::ReadContentBlockHeader` calls `UActorChannel::ReadContentBlockHeader` which does the actual `NewObject`.
This also calls `AActor::OnSubobjectCreatedFromReplication` on the assigned actor.

### Spawning replicated actor for client
The process seems to be the following:
`UNetConnection::ReceivedRawPacket` -> `UNetConnection::ReceivedPacket` -> `UNetConnection::DispatchPacket`. (See `LogNetTraffic` logs for this).

I don't know how the Actor Channel exists/is created since the representing actor doesn't exist yet.

`UChannel::ReceivedRawBunch` (bunch is of type `FInBunch`) -> `UChannel::ReceivedNextBunch` -> `UChannel::ReceivedSequencedBunch` -> `UActorChannel::ReceivedBunch` -> `UActorChannel::ProcessBunch`.
Then we simply check if the representing actor of the channel exists, if not we call `UPackageMapClient::SerializeNewActor`.

### Destroying subobject's
This seems to be done close to where they are spawned in `UActorChannel::ReadContentBlockHeader` (see `bDeleteSubObject`))

### Remapping net GUIDs
When replicated objects are spawned on server, we have to spawn them on relevant clients to.
Since they use unique net GUIDs, we have to tell to the locally spawned objects what are their net GUID.
Thats the job of `UNetDriver::UpdateUnmappedObjects`, which will call `FObjectReplicator::UpdateUnmappedObjects`  which will trigger onreps on the newly created object.

## Bunch handling
Aside what is already said in the other sections, all UObjects have a `UObject::PreNetReceive` and `UObject::PostNetReceive`. These are called on each received bunch.

## OnRep handling
The function responsible for that is `FObjectReplicator::CallRepNotifies` which calls `FRepLayout::CallRepNotifies` which uses `UObject::ProcessEvent` to run the OnRep function.

`FRepLayout::CallRepNotifies` will call `UObject::PostRepNotifies` after.
# Common Issues

## UObject not replicating
I had an issue with a UObject not replicating because it was a instanced object of another Actor Component.
Using `NewObject` and giving the object as the template fixed the issue (use the result of the function as the object to replicate). Duplicate function or changing outer didn't fix the issue.

## OnRep BP function not called in BP UObject
This is because by default the replication system won't pick up UObject replicated properties.
You have to add the following in `GetLifetimeReplicatedProps` (for example this is what the Actor class does):
```c++
if (const UBlueprintGeneratedClass* BPClass = Cast<UBlueprintGeneratedClass>(GetClass()))
{
	BPClass->GetLifetimeBlueprintReplicationList(OutLifetimeProps);
}
```

It also seems like you need to override `GetWorld`.

## OnRep OldValue
When using OldValue in `OnRep_` functions, be sure to make your parameter a reference, otherwise your value will be broken.

Example:
```c++
// broken
UFUNCTION() void OnRep_HidingActor(TWeakObjectPtr<ATFCInGameCharacter> OldVal);
// works 
UFUNCTION() void OnRep_HidingActor(TWeakObjectPtr<ATFCInGameCharacter>& OldVal);`
```
