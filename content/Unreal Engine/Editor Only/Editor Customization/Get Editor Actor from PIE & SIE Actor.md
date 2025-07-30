
See `EditorUtilities::GetEditorWorldCounterpartActor`

> [!Error] Danger
> This doesn't work if using level instances.
> The internal iterator only finds correct editor actor if the actor is in the persistent level 

**Ugly fix that works with level instances** ([full code](https://github.com/hzFishy/FishyUtils/blob/d6effb57fd8364a2ef3289c58f50a47a10ab9161/Source/FishyUtils/Public/EditorOnly/FUEditorUtilities.h#L11) for a complete version of `GetEditorWorldCounterpartActor`)
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
