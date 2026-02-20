
> [!Error] Iris notice
> Actor Channels aren't used anymore with Iris, they were "replaced" by what is called a Replication Bridge

# Resources
- See `NetDriver.h`
- See also [[NetDriver]]
- See also [[NetConnection]]

# About
Common types of channels:  
- A Control Channel is used to send information regarding state of a connection (whether or not the connection should close, etc.)  
- A Voice Channel may be used to send voice data between client and server.  
- A Unique Actor Channel will exist for every Actor replicated from the server to the client.

# Actor Channel
A channel for exchanging actor and its subobject's properties and RPCs.  
A Actor Channel manages the creation and lifetime of a replicated actor. Actual replication of properties and RPCs actually happens in `FObjectReplicator` now (see `DataReplication.h`).

There is an example of an Actor Channel bunch at the class declaration.

An actor channel has a `FObjectReplicator` called `ActorReplicator`.
A `FObjectReplicator` "Represents an object that is currently being replicated or handling RPCs.".
