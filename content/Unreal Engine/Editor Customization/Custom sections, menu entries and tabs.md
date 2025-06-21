
- Use`ToolMenus.Edit` command in editor
- See [this](https://minifloppy.it/posts/2024/adding-custom-buttons-unreal-editor-toolbars-menus/) cool post.
- See [[Unreal Engine/Slate/_Resources (Slate)|_Resources (Slate)]] for more details about reusing engine slate icons & styles.
- See [this](https://herr-edgy.com/tutorials/extending-tool-menus-in-the-editor-via-c/) for dynamic tool menu entries and to get context.
- See [this](https://zhuanlan.zhihu.com/p/628655599) cool post (use translator) about general extensions (check at the end (`FWorkflowCentricApplication` section) for a cool example of using a `FSummoner`).

# Add something to a toolbar

## Default way

==TODO==: 

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

# Tabs
See https://github.com/aquanox/SubsystemBrowserPlugin/blob/main/Source/SubsystemBrowser/SubsystemBrowserModule.cpp#L49-L76.

