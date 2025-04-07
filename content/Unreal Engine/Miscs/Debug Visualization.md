
## Custom show flag
Great [article](https://itsbaffled.github.io/posts/UE/Easier-Editor-Debug-Visualization) by Baffled.

Quick snippet
```c++
/* in editor module header */
inline const TCHAR* TargetShowFlagName = TEXT("BPG_TargetFlag");  
inline TCustomShowFlag<EShowFlagShippingValue::ForceDisabled> TargetShowFlag(TargetShowFlagName, true, SFG_Normal);

/* in editor module cpp file */
// StartupModule
TargetDebugDrawServiceDelegateHandle = UDebugDrawService::Register(BPG_Editor_Flags::TargetShowFlagName, FDebugDrawDelegate::CreateStatic(&BPGTargetDrawing::DrawTargetFlag));
// ShutdownModule
UDebugDrawService::Unregister(TargetDebugDrawServiceDelegateHandle);


/* in any regular c++ class */
// header
static void DrawTargetFlag(UCanvas* Canvas, APlayerController* PC);

// cpp file
void BPGTargetDrawing::DrawTargetFlag(UCanvas* Canvas, APlayerController* PC)  
{  
	// don't check APlayerController because it's always null
    if ( !IsValid(Canvas) || !IsValid(GWorld)) { return; };  
  
    UFont* MedFont = UEngine::GetMediumFont();  
  
    Canvas->SetDrawColor(FColor::Orange);  
    Canvas->DrawText(MedFont, TEXT("Target Show Flag Active"), 20, 100);  
    
    DrawDebugSphere(GWorld, FVector::ZeroVector, 100, 10, FColor::White);  
}
```

`TCustomShowFlag` will register the flag in `FEngineShowFlags::RegisterCustomShowFlag` as well as a custom command formatted as `ShowFlag.MyFlag`.
You can use this console command in editor and in SIE/PIE to change the state of this flag.
- `ShowFlag.MyFlag 0` will **FORCE** disable the flag
- `ShowFlag.MyFlag 1` will **FORCE** enable the flag
- `ShowFlag.MyFlag 2` will sync the flag state to the editor "Show" tab value (used by default)
