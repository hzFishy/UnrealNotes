# Core
## `FChaosEngineInterface`

## `FGenericPhysicsInterface`


# Particles
```mermaid
flowchart TD
%%{ init : {'flowchart': {'nodeSpacing': 100, 'rankSpacing': 100, "curve" : "step"}} }%%
    TPhysicsProxy --> IPhysicsProxyBase
    FSingleParticlePhysicsProxy --> IPhysicsProxyBase
    FPhysicsObject
    FGeometryCollectionPhysicsProxy --> TPhysicsProxy
    TThreadedSingleParticlePhysicsProxyBase --> FSingleParticlePhysicsProxy
    FGeometryParticleHandle[FGeometryParticleHandle TGeometryParticleHandle, TGeometryParticleHandleImp] --> TParticleHandleBase
    FSkeletalMeshPhysicsProxy --> TPhysicsProxy
```


## `FPhysicsActorHandle` (`FSingleParticlePhysicsProxy`)
Each Body Instance has a reference to a `FPhysicsActorHandle`. See [[Body Instance]].
From the `FSingleParticlePhysicsProxy` you can get its internal (`GetPhysicsThreadAPI`) and external (`GetGameThreadAPI`) rigid body handle.

It holds an `FParticleHandle`, alias of `FGeometryParticleHandle`.

## `FPhysicsObjectHandle` (`FPhysicsObject`)
The `FPhysicsObject` is effectively a reference to a single particle in the solver.  
It maintains this reference indirectly via the physics proxy. This object is meant to be usable on both the game thread and physics thread.

It holds a ref to `IPhysicsProxyBase* Proxy`, `int32 BodyIndex` and `FName BodyName`.
So for example you can have multiple `FPhysicsObject`s which has the same SKM proxy but have different body indexes and body names.

## `FGeometryParticleHandle` (`TGeometryParticleHandle`, `TGeometryParticleHandleImp`)

## `TParticleHandleBase`

## `IPhysicsProxyBase`

## `TPhysicsProxy`
Base object interface for solver objects. Defines the expected API for objects, uses CRTP for static dispatch, entire API considered "pure-virtual" and must be defined.  
Forgetting to implement any of the interface functions will give errors regarding recursion on all control paths for `TPhysicsProxy<T>` where T will be the type that has not correctly implemented the API.  

PersistentTask uses `IPhysicsProxyBase`, so when implementing a new specialized type it is necessary to include its header file in PersistentTask.cpp allowing the linker to properly resolve the new type. 

## `TThreadedSingleParticlePhysicsProxyBase`
Wrapper class that routes all reads and writes to the appropriate particle data. This is helpful for cases where we want to both write to a particle and a network buffer for example.

## `TThreadParticle` (`FGeometryParticle`, `FGeometryParticleHandle`)
It is an alias of `FGeometryParticle` if the thread is external or of type `FGeometryParticleHandle` if the thread is internal.
