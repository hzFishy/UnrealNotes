
See `SSceneOutliner`, `FActorBrowsingMode`.

You can get the folder of any actor using `GetFolder` on it

**Snippet to access the scene outliner slate widgets (up to 4 can exist)**
```c++
TWeakPtr<ILevelEditor> LevelEditor = FModuleManager::GetModuleChecked<FLevelEditorModule>("LevelEditor").GetLevelEditorInstance();  

if (LevelEditor.IsValid())  
{  
    TArray<TWeakPtr<ISceneOutliner>> SceneOutlinerPtrs = LevelEditor.Pin()->GetAllSceneOutliners();  
    
    for (TWeakPtr<ISceneOutliner> SceneOutlinerPtr : SceneOutlinerPtrs)  
    {
	    if (TSharedPtr<ISceneOutliner> SceneOutlinerPin = SceneOutlinerPtr.Pin())
	    {
		    SSceneOutliner* SceneOutliner = static_cast<SSceneOutliner*>(SceneOutlinerPin.Get());
		}  
    }
}
```

**Snippet to select all actors in the same folder as the selected actor**
```c++
// get selected actors  
TArray<AActor*> SelectedActors;  
if (GEditor->GetSelectedActors()->GetSelectedObjects<AActor>(SelectedActors) > 0)  
{  
    AActor* ReferenceActor = SelectedActors[0];  
    FFolder ReferenceFolder = ReferenceActor->GetFolder();  
    
    for (TActorIterator<AActor> It(GWorld); It; ++It)  
    {
	    if (It->GetFolder() == ReferenceFolder)
	    {
		    FU_LOG_STemp_D("Selected {0}", *It);
		    GEditor->GetSelectedActors()->Select(*It);
		}
	}
}
```
