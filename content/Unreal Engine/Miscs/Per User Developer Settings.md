
Thanks for Northstat for the example.
```cpp
UCLASS(Config = EditorPerProjectUserSettings, DefaultConfig) class UMyFrameworkLocalDevSettings : public UDeveloperSettings
```
> This will go into Editor preferences and write to `DefaultEditorPerProjectUserSettings.ini` in `Saved/` which shouldn't be committed

