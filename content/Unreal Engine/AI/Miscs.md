
# Debugging

The visual logger has a lot of AI info

# AI focus/rotation
**Rotation**
Using `SetControlRotation` on AIController wont do anything (if on the pawn you didn't explicit ask for the possesed pawn to use the controller desired rotation), you must set the actor rotation on possesed pawn.

On tick the controller calls `AAIController::UpdateControlRotation`, depending on the controller/pawn settings it will update using a specific value the rotation of the controller (and if set) the rotation of the pawn.

**Focus**
- AI focus on the AI controller determines how the AI rotates to face things. There is also a priority: default, move, gameplay. The highest priority is the one used for the rotation.
- When the AI follows a path, it uses move priority. If you set the focus with a default priority, the move will be the one chosen. If you set it as gameplay, that will win out over everything.
- See `AAIController::SetFocus` (for actor) and `AAIController::SetFocalPoint` (for position).