
## Rotate a object around another

```c++
// TestTransform is the actual position
// ReferencePointTestTransform is the "other" object
// OffsetTestTransform is a transform with just a rotation set (eg: 0, 90, 0)
FVector OffsetPos = TestTransform.GetLocation() - ReferencePointTestTransform.GetLocation();  
const FVector RotatedOffset = OffsetTestTransform.GetRotation().RotateVector(OffsetPos);  
TestTransform.SetLocation(ReferencePointTestTransform.GetLocation() + RotatedOffset);
```

## Transform world/local conversion

**World to local**
Use `Transform XXX`

**Local to world**
use `Inverse Transform XXX`