
When opening an asset using `FSimpleAssetEditor` here is a generic overview:

It will register the Details View, call `FAssetEditorToolkit::InitAssetEditor`, `FAssetEditorToolkit::RegenerateMenusAndToolbars`.

The tab manager is set in `SStandaloneAssetEditorToolkitHost::Construct` created inside `FAssetEditorToolkit::InitAssetEditor`.

The tabs registered to the tab manager are spawned from `SStandaloneAssetEditorToolkitHost::SetupInitialContent` called in `FAssetEditorToolkit::InitAssetEditor`.

For example the details tab is registered with `RegisterTabSpawner` in `FSimpleAssetEditor::RegisterTabSpawners`.

