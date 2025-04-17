Used in singleplayer and multiplayer, contains important data.

# Init
- In PIE the URL is made in `UGameInstance::StartPlayInEditorGameInstance`.
- `FUrlConfig FURL::UrlConfig` is initialized on engine init (`UEngine::Init`) with `FURL::StaticInit`.

## Portal
In `UWorld::SpawnPlayActor`, `AGameModeBase::Login`, `AGameModeBase::InitNewPlayer` the passed `Portal` (that's notably used in `AGameModeBase::FindPlayerStart_Implementation` with `PlayerStartTag` to find a player start) comes from the URL.

> [!Warning]- Warning regading `Current Camera Location`
> Setting a portal overrides this setting in PIE<br>
> ![[Pasted image 20250417202020.png]]<br>
> 
> To address this issue in PIE you can override Login in the game mode and read `PlaySettings->LastExecutedPlayModeLocation` (with `ULevelEditorPlaySettings`)


```

**To set your portal string:**
- go in `DefaultEngine.ini`
- if it doesn't already exist, add a `URL` section
- under this section, add a `Portal` key
- write your string next as the value
**Full result:**
```ini
[URL]  
Portal=StartPortal
```

