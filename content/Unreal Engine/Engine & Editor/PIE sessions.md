
# Starting PIE

## 0 - Start
When pressing play to start a PIE session(s) the editor will set data in `PlaySessionRequest`

In `UEditorEngine::Tick`, if `PlaySessionRequest.IsSet()` the engine will call 
`UEditorEngine::StartQueuedPlaySessionRequest` which calls `UEditorEngine::StartQueuedPlaySessionRequestImpl`

The latter will run checks such as:
- See if edited maps are saved
- See if we run in one process or multiple
- Broadcasting PIE editor delegates (`FEditorDelegates`)

Finally, it checks the enum value of `PlaySessionRequest->SessionDestination`
- If `EPlaySessionDestinationType::InProcess`: Create one-or-more PIE/SIE sessions inside of the current process. Calls `UEditorEngine::StartPlayInEditorSession`
- If `EPlaySessionDestinationType::NewProcess`: Create one-or-more PIE session by launching a new process on the local machine. Calls `UEditorEngine::StartPlayInNewProcessSession`
- If `EPlaySessionDestinationType::Launcher`: Create a Play Session via the Launcher which may be on a local or remote device. Calls `UEditorEngine::StartPlayUsingLauncherSession`
## 1.1 - `UEditorEngine::StartPlayInEditorSession`
`UEditorEngine::StartPlayInEditorSession` with the play settings.

Inside a lot of processes and checks are done, such as:
- Broadcasting PIE editor delegates (`FEditorDelegates`)
- Transaction checks
- For *all?* plugins check if we are in a state where a new PIE session can start
- Registry checks and flushing async loading processes
- Auto-compile dirty blueprints (if needed), show a dialog window if any errors occurred (ignored if in demo mode)
- Flush all audio sources from the editor world

Then we finally enter the PIE and multiplayer area. (See `PlayLevel.cpp l2774` as for UE5.4)
Some operations are done to see if we are simulating, need a dedicated server separated or in the same world (listen server).

*The following part will only consider the Listen Server + Client(s) route and tha twe are running in one process*
If using the listen server PIE net mode, the engine will count how many instances we want `NumRequestedInstances`.
The listen server counts as 1 client, so when looping the first instance will be using `EPlayNetMode::PIE_ListenServer`, all the following instances will be using `EPlayNetMode::PIE_Client`.
`UEditorEngine::CreateNewPlayInEditorInstance` gets called
## ? - `UEditorEngine::CreateNewPlayInEditorInstance`

Client instance gets setup, there is a special login flow if using the online subsystem (`PlayInEditorSessionInfo->bUsingOnlinePlatform`).

if not using a OSS the engine calls `OnLoginPIEComplete_Deferred`.

## ?+1 - `UEditorEngine::OnLoginPIEComplete_Deferred`

Not called directly after the call, can be called after multiple frames.

Checks if PIE logging was successful.
Calls `UEditorEngine::CreateInnerProcessPIEGameInstance` that  calls `UGameInstance::StartPlayInEditorGameInstance`. 

For the listen server it *ends* here. The editor doesn't reload the current map or load a new map.

---
But for the client, this will call an absolute`UEngine::Browse` 

The `URL`will contain the map already opened in the editor with the following IP address and port: `127.0.0.1:7777`.
**BUT** since its using the following `FURL` constructor `FURL(FURL* Base, const TCHAR* TextURL, ETravelType Type)` the `Map` property will be replaced with `UGameMapsSettings::GetGameDefaultMap()`. (so this means the PIE client will "initially" not travel to the listen server map)

Inside `UEngine::Browse`, `URL.IsInternal() && GIsClient` will succeed.
Later on this will call `UPendingNetGame::InitNetDriver`. Which calls `UIpNetDriver::InitConnect`.

*end of the client only par?*

---

At the end if the conditions are met `UEditorEngine::OnAllPIEInstancesStarted` is called.


# Switching between PIE/SIE (Eject/Possess)

This is triggered by the UI button or F8, it calls `FInternalPlayWorldCommandCallbacks::PossessEjectPlayer_Clicked`, which sets `bIsToggleBetweenPIEandSIEQueued` in `UEditorEngine` to true, eventually calling `UEditorEngine::ToggleBetweenPIEandSIE`.

to subscribe to that you have `FEditorDelegates::OnPreSwitchBeginPIEAndSIE` (called at the beginning) and `FEditorDelegates::OnSwitchBeginPIEAndSIE` (running at the end)
