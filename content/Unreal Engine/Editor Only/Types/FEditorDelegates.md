
# Map
- `FEditorDelegates::OnMapOpened` : `GWorld` will be the new loaded world.


# PIE

- `FEditorDelegates::StartPIE`:  Sent when a PIE session has been requested to Start
- `FEditorDelegates::PreBeginPIE`:  Sent when a PIE session is beginning (before we decide if PIE can run - allows clients to avoid blocking PIE)
- `FEditorDelegates::BeginPIE`:  Sent when a PIE session is beginning (but hasn't actually started yet)
- `FEditorDelegates::PostPIEStarted`:  Sent when a PIE session has fully started and after BeginPlay() has been called
- `FEditorDelegates::PrePIEEnded`:  Sent when a PIE session is ending, before anything else happens
- `FEditorDelegates::EndPIE`:  Sent when a PIE session is ending
- `FEditorDelegates::ShutdownPIE`:  Sent when a PIE session has completely shutdown
- `FEditorDelegates::PausePIE`:  Sent when a PIE session is paused
- `FEditorDelegates::ResumePIE`:  Sent when a PIE session is resumed
- `FEditorDelegates::SingleStepPIE`:  Sent when a PIE session is single-stepped
- `FEditorDelegates::OnPreSwitchBeginPIEAndSIE`:  Sent just before the user switches between from PIE to SIE, or vice-versa.  Passes in whether we are currently in SIE
- `FEditorDelegates::OnSwitchBeginPIEAndSIE`:  Sent after the user switches between from PIE to SIE, or vice-versa.  Passes in whether we are currently in SIE
- `FEditorDelegates::CancelPIE`:  Sent when a PIE session is cancelled
