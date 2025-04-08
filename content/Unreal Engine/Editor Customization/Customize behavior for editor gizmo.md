
The base class `FTypedElementViewportInteractionCustomization` holds logic *"to allow asset editors (such as the level editor) to override the base behavior of viewport interaction"*.

For example, the derived class `FActorElementLevelEditorViewportInteractionCustomization` is used when you try to move around an actor in the level.
The main function are `GizmoManipulationStarted`, `GizmoManipulationDeltaUpdate` and `GizmoManipulationStopped`.
- `GizmoManipulationStarted`
	- calls `GEditor->BroadcastBeginObjectMovement` (wrapper of `OnBeginObjectTransformEvent`)
- `GizmoManipulationStopped`
	- calls `PostEditMove` on the actor (with `bFinished` as true)
	- calls `GEditor->BroadcastEndObjectMovement` (wrapper of `OnEndObjectTransformEvent`)

