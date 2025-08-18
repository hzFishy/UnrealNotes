
# Types
- `SReferenceViewer`
- `UEdGraph_ReferenceViewer`

# Process
The `ViewReferences` is registered in `FAssetManagerEditorCommands::RegisterCommands`.
Various places (`FAssetManagerEditorModule::OnExtendContentBrowserCommands`,  `FAssetManagerEditorModule::OnExtendLevelEditorCommands` and `FAssetManagerEditorModule::OnExtendAssetEditor`) calls in the action lambda `FAssetManagerEditorModule::OpenReferenceViewerUI` (there are to overloads for this function).

When you invoke the reference viewer from an asset in the Content Browser (here I'm testing with a Blueprint Actor) the `SelectedPackages` array will contain the full path of the asset (`/Game/The/Path/To/MyBpName`) and a `FReferenceViewerParams` struct.

It will convert these paths to `FAssetIdentifier`s then call the other function overload.
If any asset was given and `OnCanOpenReferenceViewerUI` doesn't return an error `FAssetManagerEditorModule::OpenReferenceViewerTab` is called (this is where `SReferenceViewer` is created).

Then `SReferenceViewer::SetGraphRootIdentifiers` is called with the 
Asset Identifiers and the Reference Viewer Params. This will pass them to `UEdGraph_ReferenceViewer::SetGraphRoot` (A graph created for the reference viewer) and the graph will save the asset ids in `CurrentGraphRootIdentifiers`.

Still inside `SReferenceViewer::SetGraphRootIdentifiers`, `SReferenceViewer::RebuildGraph` will be called which calls `UEdGraph_ReferenceViewer::RebuildGraph` which calls `UEdGraph_ReferenceViewer::RemoveAllNodes`.
Then the reference viewer will start to populate the `Nodes` array by calling `UEdGraph_ReferenceViewer::ConstructNodes` with the `CurrentGraphRootIdentifiers`.
It will then call `UEdGraph_ReferenceViewer::RefilterGraph`.

Back on `UEdGraph_ReferenceViewer::ConstructNodes`, for each root asset identifier (for now, our Blueprint) we build a TMap where the key is our asset id and the value is a `FReferenceNodeInfo`.
We then fill the reference node info struct by calling `UEdGraph_ReferenceViewer::RebuildGraph`.
