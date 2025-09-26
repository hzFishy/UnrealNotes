For dataflow see [[Dataflow Graph]].

# Geometry Collection Component

## About the physics
All particles (also called physic objects) exists once the physic state is created.
The proxy type is `FGeometryCollectionPhysicsProxy`.
The event dispatcher is `UChaosGameplayEventDispatcher`.
The GCC inherits `IChaosNotifyHandlerInterface`.

`FGeometryCollectionItemIndex` is used to get the index of the hit proxy body from a hit (See `SetHitResultFromShapeAndFaceIndex`).

## Access to physics
- Get `FGeometryCollectionPhysicsProxy` with `GeometryCollectionComponent->GetPhysicsProxy()`
- Get Read/Write physics interface `Chaos::FReadPhysicsObjectInterface_Internal PhysicsObjectInterface = Chaos::FPhysicsObjectInternalInterface::GetRead();` (watch out there is a internal and external version, use internal if you are on the physic thread)
- Get `Chaos::FPhysicsObjectHandle` of a particle with `GCC->GetAllPhysicsObjects()`

## Collision
See `CollisionProfilePerParticle` and `CollisionProfilePerLevel`.
See `UGeometryCollectionComponent::LoadCollisionProfiles`.

## Events
GCC events such as breaks and other collision events are handled by the `EventDispatcher` (`UChaosGameplayEventDispatcher`).
It is created in the GCC constructor so there is 1 event dispatcher actor instance per GCC.
More info at [[Physics types#`UChaosGameplayEventDispatcher`]]

## Field Commands
For fields see [[Chaos Fields]]

Commands sent to `UGeometryCollectionComponent::DispatchFieldCommand` will end up in `FGeometryCollectionPhysicsProxy::BufferFieldCommand_Internal` inside a `FFieldData`.
Which is iterated inside `FGeometryCollectionPhysicsProxy::FieldParameterUpdateCallback`.
For example calling AddImpulse will result in `FieldVectorParameterUpdate` being eventually called.

## Anchor
See `SetAnchoredXXXX` functions.

## Change collision profile on broken parts/per particle
Use `SetPerParticleCollisionProfileName`. This will update `CollisionProfilePerParticle` which is read in `UGeometryCollectionComponent::LoadCollisionProfiles`.

Basic setup
```c++
void ABPGDestructibleBase::OnChaosBreakEvent(const FChaosBreakEvent& BreakEvent)
{
    if (UGeometryCollectionComponent* GCC = Cast<UGeometryCollectionComponent>(BreakEvent.Component))
{
	    const int32 ParticleIndex = BreakEvent.Index;
	    
	    // A member var of type TArray<int32> to store all broken parts over time
	    // SmallBrokenPartsCollisionProfile is of type FCollisionProfileName
	    SmallBrokenPartsBoneIds.Add(ParticleIndex);
	    GCC->SetPerParticleCollisionProfileName(SmallBrokenPartsBoneIds, SmallBrokenPartsCollisionProfile.Name);
	}
}
```

## Get particle bounds
Do `const FBox Box = PhysicsObjectInterface.GetBounds({PhysicsObject});` (there is also a `GetWorldBounds` version)

**Example of code to run on tick to draw particle bounds with their volume**
```c++
UGeometryCollectionComponent* GCC = Data.Key.Get();  
TArray<Chaos::FPhysicsObject*> PhysicsObjects = GCC->GetAllPhysicsObjects();  
for (auto& PhysicsObject : PhysicsObjects)  
{
    const FBox Box = PhysicsObjectInterface.GetWorldBounds({PhysicsObject});  
    
    const Chaos::FReal Volume = Box.GetVolume();  
    const bool bIsSmall = Volume <= 50000;  
    
    const FColor Color = bIsSmall ? FColor::Green : FColor::Red;  
    
    FU_Draw::DrawDebugBoxFrame(GetWorld(),  
       Box.GetCenter(), Box.GetExtent(), FRotator::ZeroRotator,  
       Color, 1, 1  
    );  
    
    FU_Draw::DrawDebugStringFrame(GetWorld(),  
       Box.GetCenter(), FU_Utilities::PrintCompactFloat(Volume), Color, 1
   );  
}
```

## From hit get particle
```c++
// not fully tested, WIP code

auto* Proxy = HitGCC->GetPhysicsProxy();
auto Item = VelocityHitResult.Item;

auto* ParticleAtIndex = Proxy->GetPhysicsObjectByIndex(Item);
auto* RealPart = ParticleAtIndex->GetParticle<Chaos::EThreadContext::External();
auto Loc = RealPart->GetX();
auto Rot = RealPart->GetR();

auto* RigidParticle = RealPart->CastToRigidParticle();
auto LinearVel = RigidParticle->GetV();
auto AngularVel = RigidParticle->GetW();
```

# Optimizing

## Nanite
If your mesh has a lot of polygons try to enable Nanite.

![[Pasted image 20250923165512.png]]

## Root Mesh Proxies
You can replace your GC with one or multiple static meshes until the GCC breaks.

![[Pasted image 20250923165536.png]]

## Remove On Break

![[Pasted image 20250923165821.png]]

## Remove On sleep

![[Pasted image 20250923165836.png]]

## One way Interaction

![[Pasted image 20250923170058.png]]

## Throttling Mechanisms

![[Pasted image 20250923170255.png]]

- `p.Chaos.Clustering.PerAdvanceBreaksAllowed <count>`
- `p.Chaos.Clustering.PerAdvanceBreaksRescheduleLimit <nb of frame>`

## Not using fields
Fields gives you a lot of control but has some overhead that can't be ignored in some cases.
You can use instead `Apply External Strain` and `Apply Breaking Linear Velocity`/`Apply Breaking Angular Velocity`.
You can also use internal strain if you want to apply damage over time.

# Miscs

## Make it feel heavier
See [this section](https://youtu.be/wPgd1J1Tf70?si=s_4aNtHAow4-N4yy&t=1506) of GDC 2025.

## Cause destruction from collision

![[Pasted image 20250923172053.png]]

## Niagara
Example in the content example project..

![[Pasted image 20250923172401.png]]
