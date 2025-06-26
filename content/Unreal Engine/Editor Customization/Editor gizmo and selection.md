
# Gizmo events
The base class `FTypedElementViewportInteractionCustomization` holds logic *"to allow asset editors (such as the level editor) to override the base behavior of viewport interaction"*.

For example, the derived class `FActorElementLevelEditorViewportInteractionCustomization` is used when you try to move around an actor in the level.

The main functions are `GizmoManipulationStarted`, `GizmoManipulationDeltaUpdate` and `GizmoManipulationStopped`.

- `GizmoManipulationStarted`
	- calls `GEditor->BroadcastBeginObjectMovement` (wrapper of `OnBeginObjectTransformEvent`)
- `GizmoManipulationStopped`
	- calls `PostEditMove` on the actor (with `bFinished` as true)
	- calls `GEditor->BroadcastEndObjectMovement` (wrapper of `OnEndObjectTransformEvent`)


# Selection
`USelection` is the backend of the selection flow in the editor.

Notes:
- There is `Actor::IsSelectedInEditor`
- There is `USelection::SelectionChangedEvent` and `USelection::SelectObjectEvent`
	- `SelectObjectEvent` isn't called if we clear selection with the Escape key, so if you want to handle all possibilities for an actor use `USelection::SelectObjectEvent` with `IsSelected()`.
- Use `GEditor->GetSelectedObjects()` (or other variants) to get a pointer to `USelection` (depending on the used functions some objects will be excluded from the list, for example use `GetSelectedActors` to get selected actors.)

