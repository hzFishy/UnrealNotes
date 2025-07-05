
# About

Summoner is the name given for some type of tabs, for example the `My Blueprint`, `Graph Editor`, `Construction Script Editor`. An easy example of them being used is the Blueprint Editor.
![[Pasted image 20250705144922.png]]

They are tabs BOUND to an asset.
They have special features such as:
- You can only have one tab for each of these "tab category" (See the checkmark on the left)
- You can move them around, but they will be closed if you close the bound asset editor.
- Their "existence" and docking position will be restored if you reopen the asset. Saving up time

To see how the Blueprint Editor ones are made see `BlueprintEditorTabFactories.h/.cpp` and `BlueprintEditorModes.h/.cpp` files.

# Add your custom summoners
Here are the barebones of the required setup.
Do it in an editor module.
**Here we are adding a summoner tab for the Blueprint Editor, I didn't test for other asset types.**

First, make your summoner struct type
```c++
// the summoner struct type
struct PREFABSYSTEMEDITOR_API FPSPrefabSystemBlueprintSummoner : FWorkflowTabFactory  
{

public:
	FPSPrefabSystemBlueprintSummoner(TSharedPtr<FBlueprintEditor> InBlueprintEditorPtr);
	
	virtual TSharedRef<SWidget> CreateTabBody(const FWorkflowTabSpawnInfo& Info) const override;
	
	virtual FText GetTabToolTipText(const FWorkflowTabSpawnInfo& Info) const override;
};

// summoner body in .cpp
FPSPrefabSystemBlueprintSummoner::FPSPrefabSystemBlueprintSummoner(TSharedPtr<FBlueprintEditor> InBlueprintEditorPtr)  
    : FWorkflowTabFactory("TempUniqueIdForPRefabSumoner", InBlueprintEditorPtr)  
{
	TabLabel = INVTEXT("Test tab label");
	TabIcon = PS::Editor::Helpers::GetPrefabSystemIcon();
	
    bIsSingleton = true;
	
    ViewMenuDescription = INVTEXT("ViewMenuDescription");
    ViewMenuTooltip = INVTEXT("ViewMenuTooltip");
}

TSharedRef<SWidget> FPSPrefabSystemBlueprintSummoner::CreateTabBody(const FWorkflowTabSpawnInfo& Info) const  
{
    return SNew(STextBlock).Text(INVTEXT("Prefab Summoner test"));
}

FText FPSPrefabSystemBlueprintSummoner::GetTabToolTipText(const FWorkflowTabSpawnInfo& Info) const
{
    return INVTEXT("Test tooltip");
}

```


Next, register it
```c++
// Call this after GEditor is valid, a simple way of doing that is using FCoreDelegates::OnPostEngineInit
FBlueprintEditorModule& BlueprintEditorModule = FModuleManager::LoadModuleChecked<FBlueprintEditorModule>("Kismet");
BlueprintEditorModule.OnRegisterTabsForEditor().AddLambda([] FWorkflowAllowedTabSet& TabSet, FName ModeName, TSharedPtr<FBlueprintEditor> BlueprintEditor)
{
	TabSet.RegisterFactory(MakeShared<FPSPrefabSystemBlueprintSummoner>(BlueprintEditor));
});
```

And now, its done!
<br>![[Pasted image 20250705145940.png]]<br>
![[Pasted image 20250705150003.png]]