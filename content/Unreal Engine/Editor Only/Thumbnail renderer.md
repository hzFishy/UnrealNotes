
When you open a BP for the first time on each new editor session `FAssetThumbnailPool::LoadThumbnail` will eventually be called because in `FAssetThumbnailPool::Tick`, `RealTimeThumbnailsToRender` will hold an entry for the opened BP.

This will do `ThumbnailTools::RenderThumbnail` -> `UBlueprintThumbnailRenderer::Draw` -> `FBlueprintThumbnailScene::SetBlueprint` -> `FClassActorThumbnailScene::SpawnPreviewActor` -> `UWorld::SpawnActor` -> ...

