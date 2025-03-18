
# Body Instance
See [[PrimitiveComponent]] for extra info on how they work together.

A important parent class of the BI is `FBodyInstanceCore`
The body instance seems to be mostly used from `UPrimitiveComponent`.
It also seems that a lot of physic functions are running from `FBodyInstance`.

Some of these functions will register a physic command using `ApplyAsyncPhysicsCommand`, that has a function call-back that's using `FPhysicsInterface`.

BIs are initialized using a `UBodySetup`

**Change collision:**
For some reason`SetCollisionEnabled(NewState)` doesn't work as expected, use `SetShapeCollisionEnabled(0, NewState)` instead.

**Constraint**
When you select a PrimitiveComponent, you can set Constraints <br>
![[Pasted image 20250318220101.png]]
**These constraint settings are ONLY applied on the first body instance of this component**
For a regular static mesh, this would be expected, but for a skeletal mesh component, this means only the root bone is constrained.

To apply constraint to other body instances, one way is to iterate the `Bodies` (available in SKMC) and apply set the constraint you want
Example:
```c++
// iterate all the bodies of this Static Skeletal Mesh Component
for (int i = 0; i < Bodies.Num(); ++i)  
{  
    FBodyInstance* BI = Bodies[i];  

	// Those settings are almost the same that you can see in the details panel
    BI->DOFMode = EDOFMode::Type::Default;
    // Here we are locking all axis for translation
    BI->bLockXTranslation = true;
    BI->bLockYTranslation = true;
    BI->bLockZTranslation = true;
    // Must be called so the BI create a constraint in the physic engine (it will read the variables we just set)
    BI->CreateDOFLock();
}
```
