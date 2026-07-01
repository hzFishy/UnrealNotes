
Great [article](https://itsbaffled.github.io/posts/UE/Easier-Editor-Debug-Visualization) by Baffled with a full example and more detailed code.


**Quick snippet**
```c++
/* in editor module header or cpp file*/
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

> [!info] More about `TCustomShowFlag`
> > [!Warning] Include
> > 
> > As mentioned [here](https://zomgmoz.tv/unreal/ShowFlags) *"If you define TCustomShowFlag values in a .h file, you can't include the header from more than one .cpp file, as it will cause an error. 
> > You must put TCustomShowFlag definitions in a .cpp file."*
> 
> `TCustomShowFlag` will register the flag in `FEngineShowFlags::RegisterCustomShowFlag` as well as a custom command formatted as `ShowFlag.MyFlag`.
You can use this console command in editor and in SIE/PIE to change the state of this flag.
> - `ShowFlag.MyFlag 0` will **FORCE** disable the flag
> - `ShowFlag.MyFlag 1` will **FORCE** enable the flag
> - `ShowFlag.MyFlag 2` will sync the flag state to the editor "Show" tab value (used by default)

`FEngineShowFlags::OnCustomShowFlagRegistered` is called when a custom show flag is registered.
