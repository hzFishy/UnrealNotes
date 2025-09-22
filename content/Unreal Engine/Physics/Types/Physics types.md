# Scenes

## `FChaosScene`
Low level Chaos scene used when building custom simulations that don't exist in the main world physics scene.

In constructor it creates a new solver.

## `FPhysScene_Chaos`
### About
Child of `FChaosScene`.
Low level Chaos scene used when building custom simulations that don't exist in the main world physics scene.

This is created in the `AChaosSolverActor` constructor or `UWorld::CreatePhysicsScene`.

Using `GetOwningComponent` (which uses `PhysicsProxyToComponentMap`) you can get the primitive component that created the given `IPhysicsProxyBase`. 

`FChaosScene::EndFrame` is called from `UWorld::FinishPhysicsSim`.0
### Events
It manages the collisions and other events and sends them to gameplay objects if needed.
They are registered in the constructor with the event manager and `RegisterHandler`.
The events types are:
- Collisions (see `FPhysScene_Chaos::HandleCollisionEvents`)
- Breaks (see `FPhysScene_Chaos::HandleBreakingEvents`)
- Removal (see `FPhysScene_Chaos::HandleRemovalEvents`)
- Crumbling (see `FPhysScene_Chaos::HandleCrumblingEvents`)

All primitive components that enabled `bNotifyRigidBodyCollision` are listed in the `CollisionEventRegistrations` array (so only primitives with a body instance). See `FPhysScene_Chaos::RegisterForCollisionEvents`.

All Geometry Collection Components that enabled `bNotifyGlobalCollisions` are listed in the `GlobalCollisionEventRegistrations` array. See `FPhysScene_Chaos::RegisterForGlobalCollisionEvents`.

**Collision Events**
After the solver event manager calls `FPhysScene_Chaos::HandleCollisionEvents`, which will call `FPhysScene_Chaos::HandleEachCollisionEvent`, `FPhysScene_Chaos::DispatchPendingCollisionNotifies` and `FPhysScene_Chaos::HandleGlobalCollisionEvent`.

`FPhysScene_Chaos::HandleCollisionEvents` is called from `FChaosScene::EndFrame` -> `FPBDRigidsSolver::SyncEvents_GameThread` -> `FEventManager::DispatchEvents` -> `HandleEvent` (in `TRawEventHandler`).
`HandleEachCollisionEvent` will iterate all collisions of a given body and create a new entry in the `PendingCollisionNotifies` array using `FPhysScene_Chaos::GetPendingCollisionForContactPair`.

`DispatchPendingCollisionNotifies` will use the `PendingCollisionNotifies` array that we previously filled and call `UPhysicsCollisionHandler::HandlePhysicsCollisions_AssumesLocked` on the world `PhysicsCollisionHandler` (if any) and for each collision notify call `AActor::DispatchPhysicsCollisionHit`.

`HandleGlobalCollisionEvent` will iterate the Collision Data and try to find if any collision was caused/received by a primitive component listed in the `GlobalCollisionEventRegistrations` array. If yes we make a new `FCollisionChaosEvent` entry. At the end we send all the new entries to the scene `ChaosEventRelay` by calling `UChaosEventRelay::DispatchPhysicsCollisionEvents`.


# Solvers
## `FPhysicsSolverBase`


## `FPBDRigidsSolver`
This holds an Event Manager.
It is by default created in `FChaosSolversModule::CreateSolver`, which is done in `FChaosScene` constructor.

## `AChaosSolverActor`


# Event Listeners

## `FEventManager`
Owned by a `FPBDRigidsSolver`.
Created in the constructor.

When using `RegisterHandler` we store a new `TRawEventHandler` in `EventContainers`.
Before the events are sent they need to be filled. This is done via `FPhysicsSolverAdvanceTask::AdvanceSolver` -> `FPBDRigidsSolver::AdvanceSolverBy` -> `AdvanceOneTimeStepTask::DoWork` -> `FEventManager::FillProducerData` outside the game thread. This calls `InjectProducerData` on the `TEventContainer`, which uses `InjectedFunction` and seems to be `FEventDefaults::RegisterCollisionEvent` (this is the case for collisions, the same path is used for `RegisterBreakingEvent`, `RegisterTrailingEvent`, `RegisterSleepingEvent`, `RegisterRemovalEvent`, `RegisterCrumblingEvent`).

## `FEventDefaults`
See `FEventManager`.
`FEventDefaults::RegisterSystemEvents` is called in `FPBDRigidsSolver::Reset` which is called in the `FPBDRigidsSolver` constructor.

## `FSolverEventFilters`
Container for the Solver Event Filters that have settings exposed through the Solver Actor.
Owned by a `FPBDRigidsSolver`.
Created in the constructor.

It holds a collision, breaking, trailing and removal filter.
See `FSolverCollisionEventFilter`, `FSolverBreakingEventFilter`, `FSolverTrailingEventFilter` and `FSolverRemovalEventFilter`.

## `UPhysicsCollisionHandler`
A very basic class that a world can have (created in `UWorld::InitWorld`).
By default it offers to play a sound when two rigid bodies collide with each other.

It has two important virtuals: `HandlePhysicsCollisions_AssumesLocked` and `DefaultHandleCollision_AssumesLocked`. 
The `FCollisionNotifyInfo` will contain the body index and bone for skeletal meshes, but invalid body index for Geometry Collection Components. This is because for GCCs `SetCollisionInfoFromComp` is used, which will fail to find a body instance for the GCC since it doesn't have one. While for the other types `FPhysScene_Chaos::GetPendingCollisionForContactPair` is used.

**Note:** `UPhysicsCollisionHandler::HandlePhysicsCollisions_AssumesLocked` is called from `UChaosGameplayEventDispatcher::DispatchPendingCollisionNotifies` and `FPhysScene_Chaos::DispatchPendingCollisionNotifies`.
The `UChaosGameplayEventDispatcher` will send collisions from GCCs and `FPhysScene_Chaos` for the other classic primitive collisions.

## `UChaosEventRelay`
An object managing events.
Handle collision, break, removal and crumbling events.
Created in the `FPhysScene_Chaos` constructor.

## `UChaosEventListenerComponent`
Base class for listeners that query and respond to a frame's physics data (collision events, break events, etc).

Its TickGroup is set to Post Physics.

## `UChaosGameplayEventDispatcher`
Child of `UChaosEventListenerComponent`.

List of events this event dispatcher can handle:
- `CollisionEvents` (see `FCollisionEventData` and `RegisterForCollisionEvents`)
- `BreakingEvents` (see `FBreakingEventData` and `RegisterForBreakEvents`)
- `SleepingEvents` (see `FSleepingEventData`)
- `RemovalEvents` (see `FRemovalEventData` and `RegisterForRemovalEvents`)
- `CrumblingEvents` (see `FCrumblingEventData` and `RegisterForCrumblingEvents`)

The events are then registered by the solver event manager in `UChaosGameplayEventDispatcher::RegisterChaosEvents` with `RegisterHandler`.

**Collision events**
`UChaosGameplayEventDispatcher::HandleCollisionEvents` is called from `FChaosScene::EndFrame` -> `FPBDRigidsSolver::SyncEvents_GameThread` -> `FEventManager::DispatchEvents` -> `HandleEvent` (in `TRawEventHandler`).

**Breaking events**
See `FRigidClustering`.

# Collisions

## `FCollisionEventData`
Holds a `FAllCollisionData` and `FIndicesByPhysicsProxy`.
Used for example in `UChaosGameplayEventDispatcher::HandleCollisionEvents`.

## `FAllCollisionData`
All the collision events for one frame time stamped with the time for that frame.
It holds a `FCollisionDataArray`, which is an array of `FCollidingData`

## `FCollidingData`
Collision event data stored for use by other systems (e.g. Niagara, gameplay events).

## `FRigidBodyCollisionInfo`
Information about a specific object involved in a rigid body collision.

## `FCollisionNotifyInfo`
One entry in the array of collision notifications pending execution at the end of the physics engine run.

## `FSolverCollisionEventFilter`


## `FCollisionObjectQueryParams`
Used in queries.
Holds a `int32 ObjectTypesToQuery` (Set of object type queries that it is interested in) and `FMaskFilter IgnoreMask` (Extra filtering done during object query. See declaration for filtering logic).

## `FMaskFilter` (`uint8`)
This filter allows us to refine queries (channel, object) with an additional level of ignore by tagging entire classes of objects (e.g. "Red team", "Blue team").
`if (QueryIgnoreMask & ShapeFilter != 0)` filter out.


## `FCollisionData`
Can be found in `FShapeInstanceProxy` in `CollisionData`.
Holds 2 `FCollisionFilterData`, `QueryData` and `SimData`.

## `FCollisionFilterData`
Contains 4 `uint32 WordX` variable.
- **`Word0`:** This seems to contain the Unique Actor ID (from comment in `UWorld::ComponentSweepMultiByChannel`).
- **`Word1`:** Seems to be the channel (from usage in `FCollisionResponseContainer ExtractQueryCollisionResponseContainer` where it checks it to know if we bloc, overlap or ignore). Setting this to `0xFFFF` seems to mean we collide with everything (see comment in `FActorHandle::CreateParticleHandle`). This is sometimes matching with `ObjectTypesToQuery`.
- **`Word2`:** This seems to be used in a similar way than `Word1` but to know if we do an overlap or ignore. Also mentionned with `touch`.
- **`Word3`:** This seems to contain the `EFilterFlags` flags (from `FCollisionFilterData::HasFlag` function behavior).

# Breaking

## `FRigidClustering`
The Chaos Destruction System allows artists to define exactly how geometry will break and separate during the simulations. Artists construct the simulation assets using pre-fractured geometry and utilize dynamically generated rigid constraints to model the structural connections during the simulation. The resulting objects within the simulation can separate from connected structures based on interactions with environmental elements, like fields and collisions.  

The destruction system relies on an internal clustering model (aka Clustering) which controls how the rigidly attached geometry is simulated. Clustering allows artists to initialize sets of geometry as  a single rigid body, then dynamically break the objects during the simulation. At its core, the clustering system will simply join the mass and inertia of each connected element into one larger single rigid body.  

At the beginning of the simulation a connection graph is initialized  based on the rigid body’s nearest neighbors. Each connection between the bodies represents a rigid constraint within the cluster and is given initial strain values. During the simulation, the strains within the  connection graph are evaluated. The connections can be broken when collision constraints, or field evaluations, impart an impulse on the rigid body that exceeds the connections limit. Fields can also be used to decrease the internal strain values of the connections, resulting in a weakening of the  internal structure.

The breaking events are sent when inside `FRigidClustering::AdvanceClustering` -> `FRigidClustering::BreakingModel` -> `FRigidClustering::ReleaseClusterParticles` -> `FRigidClustering::ReleaseClusterParticlesImpl` -> `FRigidClustering::SendBreakingEvent`.

## `FBreakingEventData`
Holds a `FAllBreakingData` and `FIndicesByPhysicsProxy`.

## `FAllBreakingData`
All the breaking events for one frame time stamped with the time for that frame.
It holds a `FBreakingDataArray`, which is an array of `FBreakingData`

## `FBreakingData`
BreakingData passed from the physics solver to subsystems.

## `FSolverBreakingEventFilter`


# Sleeping
## `FSleepingEventData`
It holds a `FSleepingDataArray`, which is an array of `FSleepingData`

## `FSleepingData`
Contains the proxy and sleeping state.

# Removal
## `FRemovalEventData`
Holds a `FAllRemovalData` and `FIndicesByPhysicsProxy`.

## `FAllRemovalData`
All the removal events for one frame time stamped with the time for that frame.
It holds a `FRemovalDataArray`, which is an array of `FRemovalData`

## `FRemovalData`
RemovalData passed from the physics solver to subsystems.

## `FSolverRemovalEventFilter`


# Crumbling
## `FCrumblingEventData`
Holds a `FAllCrumblingData` and `FIndicesByPhysicsProxy`.

## `FAllCrumblingData`
All the crumbling events for one frame time stamped with the time for that frame.
It holds a `FCrumblingDataArray`, which is an array of `FCrumblingData`

## `FCrumblingData`
CrumblingData passed from the physics solver to subsystems.

# Trailing

## `FSolverTrailingEventFilter`



# Materials
## `FMaterialData`
Can be found in `FShapeInstanceProxy` in `Materials`.

# Particles and shapes
See [[Particles and shapes]]

# Miscs
## `FIndicesByPhysicsProxy`
Maps PhysicsProxy to list of indices in events arrays.
For looking up all collisions a particular physics object had this frame.
The list of indices inside `FIndicesByPhysicsProxy` points to the linked array in the same struct, for example in `FCollisionEventData` this would point to the `FAllCollisionData` array.

## `FChaosEngineInterface`


## `FGenericPhysicsInterface`
