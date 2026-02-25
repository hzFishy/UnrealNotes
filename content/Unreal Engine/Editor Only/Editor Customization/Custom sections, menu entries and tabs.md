
# Resources
- Use `ToolMenus.Edit` command in editor to see the menu entry points
- See [this](https://minifloppy.it/posts/2024/adding-custom-buttons-unreal-editor-toolbars-menus/) cool post on how to add custom entries to toolbar and menu entries in menu.
- See [[Unreal Engine/Slate/_Resources (Slate)|_Resources (Slate)]] for more details about reusing engine slate icons & styles.
- See [this](https://herr-edgy.com/tutorials/extending-tool-menus-in-the-editor-via-c/) for dynamic tool menu entries and to get context.
- See [this](https://zhuanlan.zhihu.com/p/628655599) cool post (use translator) about general extensions (check at the end (`FWorkflowCentricApplication` section) for a cool example of using a `FSummoner`).
- See [this](https://easycomplex-tech.com/blog/Unreal/AssetEditor/UEAssetEditorDev-AssetEditorClassDef/) for more info on how to create custom editor classes and how they work. And the differences between `FAssetEditorToolkit` and `FWorkflowCentricApplication`.
- See [this](https://easycomplex-tech.com/blog/Unreal/AssetEditor/UEAssetEditorDev-AssetEditorLayout/) for more information on editor layouts for application modes.
- See [this](https://easycomplex-tech.com/blog/Unreal/AssetEditor/UEAssetEditorDev-AssetEditorAppMode/) for information about application modes
- See [this](https://easycomplex-tech.com/blog/Unreal/AssetEditor/UEAssetEditorDev-AssetEditorMenuToolbar/) for examples on how to add specific tabs/toolbars/menu to your custom asset
- See [[Summoners (Tabs)]]
- See [[Custom Content Browser Menu]]

# Engine menu creation
- Level Editor Toolbar: `FLevelEditorToolBar::RegisterLevelEditorToolBar`
# Add something to a toolbar

## Default way

```c++
UToolMenu* ToolbarMenu = UToolMenus::Get()->ExtendMenu("Kismet.SubobjectEditorContextMenu");  
FToolMenuSection& ToolbarSection = ToolbarMenu->AddSection("PrefabSystemSection.BlueprintEditor.Component", INVTEXT("Pefabab System"));  
  
ToolbarSection.AddDynamicEntry(...);
```

## With extender
```c++
TSharedPtr<FExtender> TabExtender = MakeShareable(new FExtender);  
TabExtender->AddToolBarExtension(  
    "Asset",  
    EExtensionHook::After,  
    nullptr,  
    FToolBarExtensionDelegate::CreateLambda([](FToolBarBuilder& ToolBarBuilder)  
    {
	    ToolBarBuilder.BeginSection("QuixelBridge");
	    {
	        ToolBarBuilder.AddToolBarButton(
		        FName("Quixel Bridge"),
		        INVTEXT("Bridge"),  
		        INVTEXT("Megascans Link with Bridge"),
		        SlateIcon(),  
		        EUserInterfaceActionType::Button,  
		        FName("QuixelBridge")
			);
		}
		ToolBarBuilder.EndSection();  
    })
);
BlueprintEditor->AddToolbarExtender(TabExtender);
```

Result:
![[Pasted image 20250621192838.png]]

# Contexts
You can get the context of a ToolbarSection if using `MyToolMenuSection.AddDynamicEntry`.

Example:
```c++
UToolMenu* ToolbarMenu = UToolMenus::Get()->ExtendMenu("Kismet.SubobjectEditorContextMenu");
FToolMenuSection& ToolbarSection = ToolbarMenu->AddSection("PrefabSystemSection.BlueprintEditor.Component", INVTEXT("PefababSystem"));
  
ToolbarSection.AddDynamicEntry(FName("PrefabReferenceComponentSubMenu"), FNewToolMenuSectionDelegate::CreateLambda([this](FToolMenuSection& InSection)  
{
	// For accuracy, this would be null because we are in a subobject context
	UBlueprintEditorToolMenuContext* BlueprintEditorContext = InSection.FindContext<UBlueprintEditorToolMenuContext>();
	// ...
```

For example here we are only showing this menu entry if the right clicked component is a `UPSPrefabReferenceComponent`:
```c++
ToolbarSection.AddDynamicEntry(FName("PrefabReferenceComponentSubMenu"), FNewToolMenuSectionDelegate::CreateLambda([this](FToolMenuSection& InSection)  
{  
    USubobjectEditorMenuContext* SubobjectEditorMenuContext = InSection.FindContext<USubobjectEditorMenuContext>();  
	if (SubobjectEditorMenuContext)
	{
		bool bCanShow = false;
		TArray<UObject*> SelectedObjects = SubobjectEditorMenuContext->GetSelectedObjects();
		for (auto& SelectedObject : SelectedObjects)
		{
			auto* Casted = Cast<UPSPrefabReferenceComponent>(SelectedObject);
			if (Casted)
			{
				bCanShow = true;
				break;
			}
		}
	InSection.AddMenuEntry(...)
	// ...
```

- `UBlueprintEditorToolMenuContext` // Appears when the section is in the "main" part of the BP Editor UI
- `UContentBrowserAssetContextMenuContext` // for an entry on the right click menu of an CB asset
- `UAssetEditorToolkitMenuContext`
- `USubobjectEditorMenuContext` // For a subobject entry (ex: component)
- `ULevelEditorMenuContext` // for a menu on the main level toolbar (only in `LevelEditor.LevelEditorToolbar` ?)

# Tabs
See https://github.com/aquanox/SubsystemBrowserPlugin/blob/main/Source/SubsystemBrowser/SubsystemBrowserModule.cpp#L49-L76.


# Other
## Combo Button

> [!Danger] Text not showing in button
> I found out that when adding a ComboButton in the level editor toolbar, the text inside the button wasn't displayed (this didn't happen in other toolbars).
> 
> The fix is to override the style on the button (which is an `FToolMenuEntry`) like so: `Button.StyleNameOverride = "CalloutToolbar";`.
> The only mention on what this style does in the source code was the following: `// This style displays button text`.



