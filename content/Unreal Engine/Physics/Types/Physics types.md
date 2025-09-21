# Scenes

## `FChaosScene`
Low level Chaos scene used when building custom simulations that don't exist in the main world physics scene.

In constructor it creates a new solver.

## `FPhysScene_Chaos`
Child of `FChaosScene`.
Low level Chaos scene used when building custom simulations that don't exist in the main world physics scene.

This is created in the `AChaosSolverActor` constructor.

It manages the collisions and other events and sends them to gameplay objects if needed.
They are registered in the constructor with the event manager and `RegisterHandler`.
The events types are:
- Collisions (see `FPhysScene_Chaos::HandleCollisionEvents`)
- Breaks (see `FPhysScene_Chaos::HandleBreakingEvents`)
- Removal (see `FPhysScene_Chaos::HandleRemovalEvents`)
- Crumbling (see `FPhysScene_Chaos::HandleCrumblingEvents`)

Using `GetOwningComponent` (which uses `PhysicsProxyToComponentMap`) you can get the primitive component that created the given `IPhysicsProxyBase`. 

# Solvers
## `FPhysicsSolverBase`


## `FPBDRigidsSolver`
This holds an Event Manager.
It is by default created in `FChaosSolversModule::CreateSolver`, which is done in `FChaosScene` constructor.

## `AChaosSolverActor`


# Event Listeners
## `FEventManager`
Owned by a `FPBDRigidsSolver`.

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

# Collisions

## `FCollisionEventData`
Holds a `FAllCollisionData` and `FIndicesByPhysicsProxy`.
Used for example in `UChaosGameplayEventDispatcher::HandleCollisionEvents`.

## `FAllCollisionData`
All the collision events for one frame time stamped with the time for that frame.
It holds a `FCollisionDataArray`, which is an array of `FCollidingData`

## `FCollidingData`
Collision event data stored for use by other systems (e.g. Niagara, gameplay events).

# Breaking
## `FBreakingEventData`
Holds a `FAllBreakingData` and `FIndicesByPhysicsProxy`.

## `FAllBreakingData`
All the breaking events for one frame time stamped with the time for that frame.
It holds a `FBreakingDataArray`, which is an array of `FBreakingData`

## `FBreakingData`
BreakingData passed from the physics solver to subsystems.

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

# Crumbling
## `FCrumblingEventData`
Holds a `FAllCrumblingData` and `FIndicesByPhysicsProxy`.

## `FAllCrumblingData`
All the crumbling events for one frame time stamped with the time for that frame.
It holds a `FCrumblingDataArray`, which is an array of `FCrumblingData`

## `FCrumblingData`
CrumblingData passed from the physics solver to subsystems.

# Particles
See [[Particles]]

# Miscs
## `FIndicesByPhysicsProxy`
Maps PhysicsProxy to list of indices in events arrays.
For looking up all collisions a particular physics object had this frame.
The list of indices inside `FIndicesByPhysicsProxy` points to the linked array in the same struct, for example in `FCollisionEventData` this would point to the `FAllCollisionData` array.

## `FChaosEngineInterface`


## `FGenericPhysicsInterface`
