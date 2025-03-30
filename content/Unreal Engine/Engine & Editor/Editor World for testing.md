Thanks to Northstar (Unreal Source Discord) for the help and snippets.

By just doing `FBPGTestWorld TestWorld` you are making a new GI and World. This allows you to do a lot of things, such as automated tests and spawning actors.

**Code to make this GI and World**
```c++
// Header
struct FBPGTestWorld  
{  
    TObjectPtr<UBPGGameInstance> GameInstance;  
    TObjectPtr<UWorld> World;  
  
public:  
    FBPGTestWorld();  
    ~FBPGTestWorld();  
  
    void Tick(float DeltaSeconds = 0.001953125);  
    void EndTick();  
  
    UWorld* operator->() const { return World.Get(); }  
};

// Cpp file
FBPGTestWorld::FBPGTestWorld() :  
    World(UWorld::CreateWorld(EWorldType::Game, true))  
{  
    check(IsInGameThread());  
  
    GameInstance = NewObject<UBPGGameInstance>();  
    GameInstance->AddToRoot();  
  
    auto& WorldContext = GEngine->CreateNewWorldContext(EWorldType::Game);  
    WorldContext.SetCurrentWorld(World);  
    World->UpdateWorldComponents(true, true);  
    World->AddToRoot();  
    World->SetFlags(RF_Public | RF_Standalone);  
    // We are responsible for ticking this world  
    World->SetShouldTick(false);  
  
    GameInstance->InitForTest(World);  
  
#if WITH_EDITOR  
    GEngine->BroadcastLevelActorListChanged();  
#endif  
    World->InitializeActorsForPlay(FURL());  
    auto* Settings = World->GetWorldSettings();  
    Settings->MinUndilatedFrameTime = 0.0001;  
    Settings->MaxUndilatedFrameTime = 10;  
  
    World->BeginPlay();  
}  
  
FBPGTestWorld::~FBPGTestWorld()  
{  
    GameInstance->RemoveFromRoot();  
    World->RemoveFromRoot();  
  
    GameInstance->Shutdown();  
  
    GEngine->DestroyWorldContext(World.Get());  
    World->DestroyWorld(true);  
  
    CollectGarbage(RF_NoFlags);  
}  
  
void FBPGTestWorld::Tick(float DeltaSeconds)  
{  
    check(IsInGameThread());  
    StaticTick(DeltaSeconds);  
    World->Tick(LEVELTICK_All, DeltaSeconds);  
    EndTick();  
  
    FTaskGraphInterface::Get().ProcessThreadUntilIdle(ENamedThreads::GameThread);  
  
    // Other things that need ticking - FTSTicker is used by FHttpModule  
    // Reference: FEngineLoop::Tick()    FTSTicker::GetCoreTicker().Tick(FApp::GetDeltaTime());  
    FThreadManager::Get().Tick();  
    GEngine->TickDeferredCommands();  
}  
  
void FBPGTestWorld::EndTick()  
{  
    check(IsInGameThread());  
    ++GFrameCounter;}
```

**This function is placed on the GI**
```c++
void UBPGGameInstance::InitForTest(UWorld* World)  
{  
    FWorldContext* TestWorldContext = GEngine->GetWorldContextFromWorld(World);  
    check(TestWorldContext);  
  
    WorldContext = TestWorldContext;  
    WorldContext->OwningGameInstance = this;  
    World->SetGameInstance(this);  
    World->SetGameMode(FURL());  
  
    Init();  
}
```