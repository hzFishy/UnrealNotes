
# Geometry Collection Component

## About the physics
All particles (also called physic objects) exists once the physic state is created.
The proxy type is `FGeometryCollectionPhysicsProxy`.
The event dispatcher is `UChaosGameplayEventDispatcher`.
The GCC inherits `IChaosNotifyHandlerInterface`.

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

