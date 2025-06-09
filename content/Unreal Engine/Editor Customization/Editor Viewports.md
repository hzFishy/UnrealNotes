
# `FViewport::Draw`
It seems that the root of any viewport drawing is this function.

> [!Info] About viewport clients
> From [this post](https://forums.unrealengine.com/t/how-to-add-a-viewport-to-custom-editor-window/416454/2) I found out that:
> - **SEditorViewport** is a slate widget to host your viewport client
> - **FEditorViewportClient** is the actual viewport class that handle the input and rendering.
> 
> Viewport clients have the same `FCommonViewportClient` base class.

Here are the various viewport client types depending on what is visible:
- Level viewport: `FLevelEditorViewportClient` (parent is `FEditorViewportClient`)
- Blueprint Viewport: `FSCSEditorViewportClient` (parent is `FEditorViewportClient`)
- Animation Viewport `FAnimationViewportClient` (parent is `FEditorViewportClient`)



# Viewports

## Get latest opened editor(s)

In this example we are only getting one editor window, but you could get all recently focused (activated) editor windows.
```C++
UAssetEditorSubsystem* AssetEditorSubsystem = GEditor->GetEditorSubsystem<UAssetEditorSubsystem>();  
TArray<UObject*> EditedAssets = AssetEditorSubsystem->GetAllEditedAssets();  
TArray<IAssetEditorInstance*> OpenedEditors;  
  
for (UObject* EditedAsset : EditedAssets)  
{
	OpenedEditors.Add(AssetEditorSubsystem->FindEditorForAsset(EditedAsset, false));  
}

IAssetEditorInstance* FocusedEditor = nullptr;  
float MaxLastActivationTime = 0.5;  

for (IAssetEditorInstance* OpenedEditor : OpenedEditors)  
{
	if (OpenedEditor && OpenedEditor->GetLastActivationTime() > MaxLastActivationTime)
	{
		MaxLastActivationTime = OpenedEditor->GetLastActivationTime();
		FocusedEditor = OpenedEditor;
	}
}  
  
FBlueprintEditor* BlueprintEditor = static_cast<FBlueprintEditor*>(FocusedEditor);
```

## Level Editor Viewport
To get the viewport of the current level you can use `GEditor->GetActiveViewport()->GetClient()`

## Blueprint Editor Viewport
The Blueprint Viewport is from `FSCSEditorViewportClient`.
As soon as one of them is visible, `FSCSEditorViewportClient::Draw` is called (See `FViewport::Draw`).

