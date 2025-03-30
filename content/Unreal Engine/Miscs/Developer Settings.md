
# User Game Developer Settings
See `UGameUserSettings`.


# Per Editor User Developer Settings
Thanks for Northstar for the example.
```cpp
UCLASS(Config = EditorPerProjectUserSettings, DefaultConfig) class UMyFrameworkLocalDevSettings : public UDeveloperSettings
```
> This will go into Editor preferences and write to `DefaultEditorPerProjectUserSettings.ini` in `Saved/` which shouldn't be committed


# Miscs

## Custom category

To have a custom `Config/DefaultMyCustomCategory` you can do `UCLASS(Config=MyCustomCategory, defaultconfig)`.

To make the custom category name in the editor/project settings, override `CategoryName` in the developer settings constructor.

## Saving Developer Settings manually
If you are editing the CDO using a mutable ptr, you must use `TryUpdateDefaultConfigFile()` to write to `.ini` file.
This works for Developer Settings classes with `defaultconfig` in the `UCLASS`

