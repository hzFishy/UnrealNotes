
When you open an asset (like a Blueprint) which has a viewport, a new world is created.

The world is initialized by the `FPreviewScene` in the constructor (`FPreviewScene::FPreviewScene`) using `UWorld::InitializeNewWorld`.

The `FSCSEditorViewportClient::Tick` ticks the preview scene's world. 

See also [[Thumbnail renderer]]
# Actor & Components in preview
The preview actor is made on first tick (`FBlueprintEditor::Tick` -> `FBlueprintEditor::UpdatePreviewActor`).
It is spawning the actor from the `GeneratedClass`  of the Blueprint.
You can get this preview actor instance from the Simple Construction Script with `GetComponentEditorActorInstance`

The components are made by the SCS in `USimpleConstructionScript::ExecuteScriptOnActor` by using the `USCS_Node::ExecuteNodeOnActor` calls, which are called on child nodes for each given node.

This function uses `AActor::CreateComponentFromTemplate` to construct the actual `UActorComponent` object.


# Refresh/Invalidation
When you do things in the Blueprint Editor (ex: in the UI, Compiling) or when you reopen the viewport tab `FSCSEditorViewportClient::InvalidatePreview` will be called.

This will call `FBlueprintEditor::UpdatePreviewActor`. Which calls `AActor::ReregisterAllComponents` and `AActor::RerunConstructionScripts`.
This will run again the SCS and spawn the components as explained in the above section.


