
# Types

## `UEdMode`
Base class for all editor modes.

## Misc Entry points
- Get the `FTabManager` representing the Editor Modes with `FLevelEditorModule::GetLevelEditorTabManager`
	- Snippet: `FLevelEditorModule& LevelEditorModule = FModuleManager::GetModuleChecked<FLevelEditorModule>("LevelEditor")` and `TSharedPtr<FTabManager> LevelEditorTabManager = LevelEditorModule.GetLevelEditorTabManager()`.
- Get `FEditorModeTools` with `SLevelEditor::GetEditorModeManager` or `GLevelEditorModeTools`.
- Get `FEditorModeRegistry` with the static `Get` method.

# Editor Mode Anatomy
Each mode has a `Toolkit` and commands.

# Add custom Editor Mode
- See `Sample Tools Editor Mode` plugin which is very small and contains a few example on how to implement a new editor mode in UE5+.

But basically you make a subclass of `UEdMode` and `FModeToolkit` in a editor module. As well as a `TCommand` class that you will register in your module.

In your toolkit class implement `GetInlineContent` so show a widget in the empty panel.

# Engine Registration of Editor Modes
When `FCoreDelegates::OnAllModuleLoadingPhasesComplete` is broadcasted `UAssetEditorSubsystem::RegisterEditorModes` is run.

This will iterate all `UEdMode` objects.
After a few checks and if the mode isn't already registrated, we add it to the `EditorModes` array.

`FEditorModeRegistry::CreateMode` is used for legacy modes going through `ULegacyEdModeWrapper::CreateLegacyMode`.

# Engine Creation Editor Mode
When `SLevelEditor::RefreshEditorModeCommands` is called and will handle mapping actions which triggers `SLevelEditor::ToggleEditorMode` on editor UI click.

# Engine Changing Editor Mode flow
When switching Editor Mode `SLevelEditor::ToggleEditorMode` is called with the mode ID.
This will eventually call `FEditorModeTools::ActivateMode`

`FEditorModeTools` holds an array of currently active editor modes (See `ActiveScriptableModes` and `IsModeActive`/`GetActiveScriptableMode`).

If it can't "recycle" the mode that needs to be activated, it will call `UAssetEditorSubsystem::CreateEditorModeWithToolsOwner`, which looks an internal `EditorModes` array.

It the mode was changed `EditorModeIDChangedEvent` delegate is broadcasted.
