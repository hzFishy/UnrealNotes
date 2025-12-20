
# Create
You can create a body setup from scratch, this is especially useful when yo uare also creating new body instances from scratch.

```c++
// Outet should be a primitive component or an asset linked to a physics representation
UBodySetup* NewBodySetup = NewObject<UBodySetup>(this);
NewBodySetup->DefaultInstance.SetCollisionProfileName(UCollisionProfile::BlockAll_ProfileName);

FKAggregateGeom Geom;

FKSphereElem TestSphere;
TestSphere.Center = FVector(0, 0, 0);
TestSphere.Radius = 20;
Geom.AddElement(TestSphere);

NewBodySetup->AddCollisionFrom(Geom);
```
