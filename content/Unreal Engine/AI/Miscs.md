
# AI not playing animations
enable `bUseAccelerationForPaths` on the CMC

# AI braking
to change the AI braking you have on the Nav Component (can be CMC)
- `bUseFixedBrakingDistanceForPaths`
- `FixedPathBrakingDistance`

# AI controller and rotation
Using SetControlRotation on AIController wont do anything (if on the pawn you didn't explicit ask for the possesed pawn to use the controller desired rotation), you must set the actor rotation on possesed pawn.


# AI focus/rotation
**Rotation**
On tick the controller calls `AAIController::UpdateControlRotation`

**Focus**
- AI focus on the AI controller determines how the AI rotates to face things. There is also a priority: default, move, gameplay. The highest priority is the one used for the rotation.
- When the AI follows a path, it uses move priority. If you set the focus with a default priority, the move will be the one chosen. If you set it as gameplay, that will win out over everything.
- See `AAIController::SetFocus` (for actor) and `AAIController::SetFocalPoint` (for position).