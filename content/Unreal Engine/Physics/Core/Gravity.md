
## Cancel gravity force on slope

Used on a capsule, the capsule has no friction.

Code
```c++
FVector Normal = CachedGroundSweepResult.ImpactNormal;  
  
// Get slope direction (tangent vector pointing down the ramp)  
FVector SlopeDirection = FVector::CrossProduct(Normal, FVector::CrossProduct(Normal, FVector::UpVector));  
SlopeDirection = SlopeDirection.GetSafeNormal(); // this is the direction gravity tries to move the capsule  
  
// Project gravity along the slope direction  
float GravityStrength = GetWorld()->GetGravityZ();  
float GravityAlongSlope = FVector(0,0,GravityStrength).ProjectOnTo(SlopeDirection).Size();  
  
// Equal and opposite force  
FVector CancelForce = -SlopeDirection * GravityAlongSlope * PlayerCapsuleBodyInstance->GetBodyMass();  
  
PlayerCapsuleBodyInstance->AddForce(CancelForce, true, false);
```


## Custom gravity
[Custom Gravity in UE 5.4](https://dev.epicgames.com/community/learning/tutorials/w6l7/unreal-engine-custom-gravity-in-ue-5-4)

