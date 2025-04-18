
See `EditorUtilities::GetEditorWorldCounterpartActor`

> [!Error] Danger
> This doesn't work if using level instances.
> The internal iterator only finds correct editor actor if the actor is in the persistent level 

**Ugly fix that works with level instances**
```c++
UWorld* EditorWorld = GEditor->EditorWorld;
for (auto LevelIt(EditorWorld->GetLevelIterator()); LevelIt; ++LevelIt)  
{
	if (const ULevel* Level = *LevelIt)
	{
		UWorld* World = CastChecked<UWorld>(Level->GetOuter());
		for (TActorIterator<ABPGCableManager> It(World); It; ++It)  
		{
			if (It->GetActorLabel().Equals(this->GetActorLabel()))
			{ 
				It->SetSavedDataFromPIE(SavedBodyInstances);
				return;
			}
		}
	}
}
```
