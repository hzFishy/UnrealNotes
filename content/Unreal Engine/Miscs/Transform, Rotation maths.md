
# Resources
- [Make Rot from X? Y? Z?](https://make-study.hatenablog.com/entry/2024/05/16/143133) (Japanese, use translator)

## Rotate an object around another

```c++
// here in this example we are running this code on tick
// RelativeLocation is 100, 0, 0 in this example.

Rotation += FRotator(0, 1, 0);
const FVector CenterLocation = GetActorLocation();
RotatedLocation = CenterLocation + Rotation.RotateVector(RelativeLocation);
```

In red its `CenterLocation` and in green `RotatedLocation`
<br>![[RotationMath.gif]]

## Transform world/local conversion

**World to local**
Use `Transform XXX`

**Local to world**
use `Inverse Transform XXX`