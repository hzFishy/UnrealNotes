
# Geometry Collection Component

## About the physics
All particles (also called physic objects) exists one the physic state is created.

## Access to physics
- Get `FGeometryCollectionPhysicsProxy` with `GeometryCollectionComponent->GetPhysicsProxy()`
- Get Read/Write physics interface `Chaos::FReadPhysicsObjectInterface_Internal PhysicsObjectInterface = Chaos::FPhysicsObjectInternalInterface::GetRead();` (watch out there is a internal and external version)
- Get `Chaos::FPhysicsObjectHandle` of a particle with `GCC->GetAllPhysicsObjects()[ParticleIndex]`

## Change collision profile on broken parts/per particle
Use `SetPerParticleCollisionProfileName`
Basic setup
```c++
void ABPGDestructibleBase::OnChaosBreakEvent(const FChaosBreakEvent& BreakEvent)  
{  
    if (UGeometryCollectionComponent* GCC = Cast<UGeometryCollectionComponent>(BreakEvent.Component))  
    {
	    // TArray<int32>
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

