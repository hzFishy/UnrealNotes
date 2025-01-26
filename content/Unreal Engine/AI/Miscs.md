
# AI not playing animations
enable `bUseAccelerationForPaths` on the CMC

# AI braking
to change the AI braking you have on the Nav Component (can be CMC)
- `bUseFixedBrakingDistanceForPaths`
- `FixedPathBrakingDistance`

# AI controller and rotation
using SetControlRotation on AIController wont do anything, you must set the actor rotation on possesed pawn