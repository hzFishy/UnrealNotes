
To edit the default new blueprint dialog class you can use the following in `DefaultEditor.ini`:
```ini
[/Script/UnrealEd.UnrealEdOptions]
; Here we add TFCUserWidget class (UTFCUserWidget)
+NewAssetDefaultClasses=(ClassName="/Script/MyModule.TFCUserWidget", AssetClass="/Script/Engine.Blueprint")
```
more info [here](https://zomgmoz.tv/unreal/Editor-customization/Customize-the-New-Blueprint-dialog#manual-setup)

To edit the new widget dialog you can edit `FavoriteWidgetParentClasses` in the `DefaultEditor.ini` or in the project settings
```init
[/Script/UMGEditor.UMGEditorProjectSettings]
+FavoriteWidgetParentClasses=/Script/MyModule.TFCUserWidget
```
