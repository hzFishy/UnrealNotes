# Resources
- See `FEditorFileUtils` in `FileHelpers.h`

# General save flow
If `FContentBrowserSingleton::CreateModalSaveAssetDialog` was used, `FModalResults::OnObjectPathChosenForSave` will be called once the user closed (canceled) the save dialog or when they pressed "Save".

# Level

If a level, this will continue to `FLevelEditorActionCallbacks::SaveCurrentAs`.
And eventually it reaches `FEditorFileUtils::SaveMap`.
