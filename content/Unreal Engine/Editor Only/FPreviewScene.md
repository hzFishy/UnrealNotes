
When you open an asset (like a Blueprint) which has a viewport, a new world is created.

The world is initialized by the `FPreviewScene` in the constructor (`FPreviewScene::FPreviewScene`) using `UWorld::InitializeNewWorld`.


The `FSCSEditorViewportClient::Tick` ticks the preview scene's world. 