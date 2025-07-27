**More info at the types declarations**

# Archive & `FBitWriter` types
## `FNetBitWriter`
A bit writer that serializes FNames and UObject* through a network packagemap.

## `FNetBitReader`
A bit reader that serializes FNames and UObject* through a network packagemap.

# Bridge types
## `UEngineReplicationBridge`
N/A

## `UObjectReplicationBridge`
Partial implementation of ReplicationBridge that can be used as a foundation for implementing support for replicating objects derived from UObject.

## `UReplicationBridge`
N/A.


# Interfaces
## `IInterface_ActorSubobject`
Interface for actor subobjects, used if the subobject isn't a actor component.
Marked as experimental but still used in engine so should be fine.

# Miscs

## `FNetworkGUID`
Implements a globally unique identifier for network related use.

## `UPackageMap`
Maps objects and names to and from indices for network communication.

## `UReplicationSystem`
N/A

## `FReplicationSystemUtil`
Helper methods to interact with Replication System from engine code

## `FRepLayout`
This class holds all replicated properties for a given type (either a `UClass`, `UStruct`, or `UFunction`).
Helpers functions exist to read, write, and compare property state.
There is only one `FRepLayout` for a given type, meaning all instances of the type share the `FRepState`.

## `FRepState`
Replication State that is unique Per Object Per Net Connection.

## `FReplicationChangelistMgr`
Manages a list of change lists for a particular replicated object that have occurred since the object started replicating.
Once the history is completely full, the very first changelist will then be merged with the next one (freeing a slot).
This way we always have the entire history for join in progress players.
This information is then used by all connections, to share the compare work needed to determine what to send each connection.
Connections will send any changelist that is new since the last time the connection checked.

## `FObjectReplicator`
Represents an object that is currently being replicated or handling RPCs.

## `FNetHandle` & `FNetRefHandle`
`FNetHandle` can be used to uniquely identify a replicated object for the lifetime of the application.

# `FLifetimeProperty`
This class is used to track a property that is marked to be replicated for the lifetime of the actor channel.  
This doesn't mean the property will necessarily always be replicated, it just means:  "check this property for replication for the life of the actor, and I don't want to think about it anymore".
A secondary condition can also be used to skip replication based on the condition results.

This is what `GetLifetimeReplicatedProps` uses.
