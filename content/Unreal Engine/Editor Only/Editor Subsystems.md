
# UAssetEditorSubsystem

The `UAssetEditorSubsystem` subsystem is very useful to get some data/events linked to assets.

Its initialized in `UEditorEngine::InitializeObjectReferences` (called in `FEngineLoop::Init` -> `UUnrealEdEngine::Init`).

Example on how to open a BP from a sub object component object
```c++
auto* AssetSS = GEditor->GetEditorSubsystem<UAssetEditorSubsystem>();
AssetSS->OpenEditorForAsset(OutSelectedObject, AssetTypeActivationOpenedMethod::Edit);
```

# UEditorValidatorSubsystem
Manages validation.
