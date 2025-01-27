
# AI not playing animations
enable `bUseAccelerationForPaths` on the CMC

# AI braking
to change the AI braking you have on the Nav Component (can be CMC)
- `bUseFixedBrakingDistanceForPaths`
- `FixedPathBrakingDistance`

# AI controller and rotation
Using SetControlRotation on AIController wont do anything (if on the pawn you didn't explicit ask for the possesed pawn to use the controller desired rotation), you must set the actor rotation on possesed pawn.
