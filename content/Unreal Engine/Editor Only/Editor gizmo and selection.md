
# Gizmo events
The base class `FTypedElementViewportInteractionCustomization` holds logic *"to allow asset editors (such as the level editor) to override the base behavior of viewport interaction"*.

For example, the derived class `FActorElementLevelEditorViewportInteractionCustomization` is used when you try to move around an actor in the level.
For components `FComponentElementLevelEditorViewportInteractionCustomization` is used.

The main functions are `GizmoManipulationStarted`, `GizmoManipulationDeltaUpdate` and `GizmoManipulationStopped`.

- `GizmoManipulationStarted`
	- calls `GEditor->BroadcastBeginObjectMovement` (wrapper of `OnBeginObjectTransformEvent`)
- `GizmoManipulationDeltaUpdate`
	- This is what updates the moved/rotated/scaled object (for example actor).
	- It can run `AActor::EditorApplyRotation`, `AActor::EditorApplyTranslation` and `AActor::EditorApplyScale`
- `GizmoManipulationStopped`
	- calls `PostEditMove` on the actor (with `bFinished` as true)
	- calls `GEditor->BroadcastEndObjectMovement` (wrapper of `OnEndObjectTransformEvent`)

> [!Info] Component transform tracking
> You can use `UEngine::OnComponentTransformChanged` for general updates.
> And `UEditorEngine::OnEndObjectMovement` to know when the movement finished.

# Selection

## Core
`USelection` is the backend of the selection flow in the editor.

There is `USelection::SelectionChangedEvent` and `USelection::SelectObjectEvent`.

> [!Warning] `SelectionChangedEvent` VS `SelectObjectEvent`
> `SelectObjectEvent` isn't called if we clear selection with the Escape key, so if you want to handle all possibilities for an actor use `USelection::SelectObjectEvent` with `IsSelected()`.

Use `GEditor->GetSelectedObjects()` (or other variants) to get a pointer to `USelection` (depending on the used functions some objects will be excluded from the list, for example use `GetSelectedActors` to get selected actors.)

`UUnrealEdEngine::edactSelectAll` is called when doing Select All.
More can be found in `UUnrealEdEngine` <br>
![[Pasted image 20250717120918.png]]
## Actors
There is `Actor::IsSelectedInEditor`.

Manually mark an actor as selected with 
```c++
GEditor->GetSelectedActors()->Select(Actor);
```

## Components
There is `UActorComponent::IsSelectedInEditor`

Manually mark an actor component as selected with 
```c++
GEditor->GetSelectedComponents()->Select(Component);
```


## Selection process
So, how does the `USelection` knows when we click on a component ?

See [[HitProxy]]
### In Level Editor Viewport
When you click, after some `FSlateApplication` functions ran, `SViewport::OnMouseButtonUp` is called.
This will run `FSceneViewport::OnMouseButtonUp` -> `FLevelEditorViewportClient::InputKey` -> `FEditorViewportClient::Internal_InputKey` -> `FEditorViewportClient::ProcessClickInViewport`.

Inside `FEditorViewportClient::ProcessClickInViewport`, we are using `FViewport::GetHitProxy`, which returns a `HHitProxy` (which is basically the hitbox of a component in a viewport). Then we call `FLevelEditorViewportClient::ProcessClick` -> `LevelViewportClickHandlers::ClickElement` (now we have a `FTypedElementHandle`) which will notify the `UTypedElementSelectionSet` that the selection changed.

### In Blueprint Editor Viewport.

When you click, after some `FSlateApplication` functions ran, `SViewport::OnMouseButtonUp` is called.

Then we have `FSCSEditorViewportClient::InputKey` -> `FEditorViewportClient::Internal_InputKey` -> `FEditorViewportClient::ProcessClickInViewport` -> `FSCSEditorViewportClient::ProcessClick`.

More stuff happens next (like selecting the node in the Subobject tree view) but the important part is that `USelection` isn't taken into consideration here.

So, if you spawn yourself an actor inside a blueprint viewport, a hit proxy IS created for you, but nothing will be selected.
This is because inside `FSCSEditorViewportClient::ProcessClick`, `ActorProxy->Actor == PreviewActor` will fail (since the preview actor is the default BP actor, and yours (`ActorProxy->Actor`) is just another instance you spawned yourself).
