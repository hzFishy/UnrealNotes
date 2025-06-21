
When you open an asset (like a Blueprint) which has a viewport, a new world is created.

The world is initialized by the `FPreviewScene` in the constructor (`FPreviewScene::FPreviewScene`) using `UWorld::InitializeNewWorld`.

The `FSCSEditorViewportClient::Tick` ticks the preview scene's world. 

See also [[Thumbnail renderer]]
# Components in preview
The preview actor is made on first tick (`FBlueprintEditor::Tick` -> `FBlueprintEditor::UpdatePreviewActor`).

The components are made by the SCS in `USimpleConstructionScript::ExecuteScriptOnActor` by using the `USCS_Node::ExecuteNodeOnActor` calls, which are called on child nodes for each given node.

This function uses `AActor::CreateComponentFromTemplate` to construct the actual `UActorComponent` object.

