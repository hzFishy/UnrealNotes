**PhysX isn't taken (fully) into account, only Chaos as we are taking into considerations UE5+ versions**

## Profiles
Profiles can be created in a unlimited amount, so be free to use it at anytime a specific "thing" with a specific set of collisions rules exists more than once.

## Collision rules
The Engine takes the lowest collision rule.

> [!Info]- Example
> If i got 2 object types, ObjectA and ObjectB. ObjectA wants to block ObjectB, but ObjectB ignores ObjectA : then both can overlap, because Ignore is lower than Block.

## Custom collisions per instance(s)

## Simple move
Like traces you can give a list of actors/components to ignore **WHEN MOVING** to any `UPrimitiveComponent`. This works on not-simulated physic objects.

## For physics & traces

### About
You can see everything (?) that the physic engine takes into account for filtering collisions with a body instance at `CreateShapeFilterData` (in `PhysicsFiltering.h`) called in `FBodyInstance::BuildBodyFilterData`.

it looks like the engine has a simulation filter (`FPhysicsInterface::SetSimulationFilter`) and a query filter (`FPhysicsInterface::SetQueryFilter`), used in `FBodyInstance::UpdatePhysicsFilterData`.
There is also `EFilterFlags`, used in `FCollisionFilterData` (`HasFlag` is interesting.)

For custom collision that the physic engine takes into account, there is multiple paths.

### `FMaskFilter MaskFilter`
A custom mask (see `FMaskFilter MaskFilter` in the body instance). You can also access it from the `UPrimitiveComponent`
- [Example of use with sweeps](https://forums.unrealengine.com/t/how-ignore-mask-in-collision-query-params-work/377985/8?u=hzfishy)

> [!Warning]- Max used bits of `FMaskFilter`
> It seems to be 6 if we check `NumExtraFilterBits` in `EngineTypes.h`.
> 
> And the explanation sits in the comment you can see in `GetQueryData`/`GetSimData` in `FPhysicsFilterBuilder`.
> There is also `NumCollisionChannelBits` and `NumFilterDataFlagBits` (which mentions the 32 limit).
> 
> `Word3` is an `uint32`, and the complete composition of it is `Our custom 6 bits (called ExtraFilter) + 5 bits (for Body Channel, casted to ECollisionChannel) + 21 bits (other flags) = 32`

### Contact Modify
The flag is set in `FPhysicsFilterBuilder.Word3` using `ConditionalSetFlags` with `EPDF_ModifyContacts`. It's used in the various function `FPhysicsFilterBuilder` has.

> [!warning]- `bContactModification` in the body instance
> There is a mention about contact modification at `EFilterFlags::ModifyContacts`, saying that the flag is "Unused" and "handled in Chaos callbacks". 
> 
**Meaning that setting `bContactModification` in the body instance doesn't matter for the sim callback to be used.**

In `ISimCallbackObject` you have `OnContactModification_Internal` from `ContactModification_Internal`. [See post about it](https://forums.unrealengine.com/t/chaos-contact-modification/517748/3?u=hzfishy).

> [!info]- Use cases in source
> The CMC seems to somehow use this system with `FCharacterMovementComponentAsyncCallback` by using `TSimCallbackObject`, same for `FChaosVehicleManager` using `FChaosVehicleManagerAsyncCallback`.

> [!error] `ISimCallbackObject` methods can be called on any thread.
> So be careful when using game thread only stuff.
> For example, don't use `xxx__AssumesLocked` methods (at least directly without safe checks)

The `Chaos::FCollisionContactModifier& Modifier` received in `OnContactModification_Internal` contains a list of `Constraints` (Lot's of details at `FPBDCollisionConstraint` declaration).

This list contains all the contact between all bodies in a scene. Not only Object1 and Object2 pair.
But in each contact you can get the Object1 and Object2 by using `GetParticlePair` (or by using the particle index `0` or `1` for some functions).

This list excludes sleeping bodies (even if in contact with something).

> [!info]- Contact points prediction
> From the results I had after drawing the contact points each frame, the following frames shows that the points seems "predicted" if close enough to collide in the near future. <br>
> ![[DrawPhyContacts_Box_Pred2.png]]  ![[DrawPhyContacts_Box_Pred1.png]]
> 

When using functions like `GetWorldContactLocations` you are getting the list of `FManifoldPoint`s. A collision constraint can get up to 4 of these (use `GetNumContacts` to get the count).

> [!info]- Drawing contact points (manifold points)
> <br>**Simulated sphere**<br>
> ![[DrawPhysContactPoints_Sphere.gif]]
> <br>**Simulated boxes**<br>
![[DrawPhysContactPoints_Boxes.gif]]

**Code snippets**
```c++
// minimal implementation of sim callback
struct FBPGCableBodySimCallbackInput : public Chaos::FSimCallbackInput  
{  
	// MUST have
    void Reset() {};  
};  
  
struct FBPGCableBodySimCallbackOutput : public Chaos::FSimCallbackOutput  
{  
	// MUST have
    void Reset() {};  
};  

// I don't know if ESimCallbackOptions::ContactModification is 100% required
class FBPGCableBodySimCallback : public Chaos::TSimCallbackObject<FBPGCableBodySimCallbackInput, FBPGCableBodySimCallbackOutput, Chaos::ESimCallbackOptions::ContactModification>  
{  
  
private:  
	// MUST have
    virtual void OnPreSimulate_Internal() override {};  

	// What we want
    virtual void OnContactModification_Internal(Chaos::FCollisionContactModifier& Modifier) override;  
};


// body of OnContactModification_Internal
void FBPGCableBodySimCallback::OnContactModification_Internal(Chaos::FCollisionContactModifier& Modifier)  
{  
	// Get Phys Solver and Scene
    Chaos::FPhysicsSolverBase* BaseSolver = GetSolver();
    Chaos::FPhysicsSolver* RealSolver = static_cast<Chaos::FPhysicsSolver*>(BaseSolver);
    FPhysScene_Chaos* PhysScene = static_cast<FPhysScene_Chaos*>(RealSolver->PhysSceneHack);

	// iterating all contacts
    for (Chaos::FContactPairModifier& ContactPairModifier : Modifier)  
    {
	    // Example 0
	    // We get the 2 particles of this pair
	    Chaos::TVec2<Chaos::FGeometryParticleHandle*> Particles = ContactPairModifier.GetParticlePair();
		
	    // Example 1
        // here no collisions will happen between the bodies
		ContactPairModifier.Disable();
        
		// Example 2
		// Here we are drawing the location of all the contact points
		// (can easily reach draw limit so watch out)
		if (UWorld* World = PhysScene->GetOwningWorld())  
		{
			for (int i = 0; i < ContactPairModifier.GetNumContacts(); ++i)  
		    {
				Chaos::FVec3 WorldPos0, WorldPos1;
			    ContactPairModifier.GetWorldContactLocations(i, WorldPos0, WorldPos1);
		         
			    BPG_Debug::DrawDebugSphere(World, WorldPos0, 10, FColor::Red, 2, 1);  
			    BPG_Debug::DrawDebugSphere(World, WorldPos1, 10, FColor::Blue, 2, 1);  
		    }
		}
		
		// Example 3
		// Get Body Instance from particle
		FBodyInstance* BI0 = PhysScene->GetBodyInstanceFromProxy(Particles[0]->PhysicsProxy());
    }
}


// init callback somewhere
if (UWorld* World = CapsuleComponent->GetWorld())  
{  
    if (FPhysScene_Chaos* PhysScene = World->GetPhysicsScene())  
    {
        SimCallback = PhysScene->GetSolver()->CreateAndRegisterSimCallbackObject_External<FBPGCableBodySimCallback>();
    }
}    
```


## `CollisionEnabled` in depth
From what I've found in the source code, there **is** an actual performance change depending on the `ECollisionEnabled::Type` value set on your component.
Full details:
- **No Collision**:
	- Will not create any representation in the physics engine. Cannot be used for spatial queries (raycasts, sweeps, overlaps) or simulation (rigid body, constraints). 
	- Best performance possible (especially for moving objects)  
- **Query Only**:
	- Only used for spatial queries (raycasts, sweeps, and overlaps). Cannot be used for simulation (rigid body, constraints). Useful for character movement and things that do not need physical simulation.
	- Performance gains by keeping data out of simulation tree.  
- **Physics Only**:
	- Only used only for physics simulation (rigid body, constraints). Cannot be used for spatial queries (raycasts, sweeps, overlaps). Useful for jiggly bits on characters that do not need per bone detection.
	- Performance gains by keeping data out of query tree  
- **Collision Enabled**:
	- Can be used for both spatial queries (raycasts, sweeps, overlaps) and simulation (rigid body, constraints).  

# Resources & tips
- [Collision Data in UE5: Practical Tips for Managing Collision Settings & Queries | Unreal Fest 2023](https://www.youtube.com/watch?v=xIQI6nXFygA)
