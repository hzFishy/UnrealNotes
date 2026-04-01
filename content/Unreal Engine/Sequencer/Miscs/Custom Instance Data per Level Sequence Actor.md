
This is very useful if you want to have custom per instance data, by default UE has a default instance data class which allows you to override the world origin of your sequence your are playing.

> [!Warning] Warning 
> The TransformOrigin doesn't handle scale.

Simple raw implementation:
```c++
// Most of the spawning and initializing parts are from the sequencer BP function library.
FActorSpawnParameters SpawnParams;
	SpawnParams.SpawnCollisionHandlingOverride = ESpawnActorCollisionHandlingMethod::AlwaysSpawn;
	SpawnParams.ObjectFlags |= RF_Transient;
	SpawnParams.bAllowDuringConstructionScript = true;
	
	// Defer construction for autoplay so that BeginPlay() is called
	SpawnParams.bDeferConstruction = true;
	
	LevelSequenceActor = GetWorld()->SpawnActor<ALevelSequenceActor>(SpawnParams);
	
	FMovieSceneSequencePlaybackSettings PlaybackSettings;
	PlaybackSettings.bAutoPlay = true;
	PlaybackSettings.PlayRate = PlayRate;
	LevelSequenceActor->PlaybackSettings = PlaybackSettings;
	LevelSequenceActor->GetSequencePlayer()->SetPlaybackSettings(PlaybackSettings);
	
	LevelSequenceActor->SetSequence(LevelSequence.LoadSynchronous());
	
	LevelSequenceActor->bOverrideInstanceData = true;
	auto* InstanceData = Cast<UDefaultLevelSequenceInstanceData>(LevelSequenceActor->DefaultInstanceData);
	// there is also an actor version
	InstanceData->TransformOrigin = GetInstanceTransform();
	
	LevelSequenceActor->InitializePlayer();
	
	FTransform DefaultTransform;
	LevelSequenceActor->FinishSpawning(DefaultTransform);
	
	LevelSequencePlayer = LevelSequenceActor->GetSequencePlayer();
	LevelSequencePlayer->OnFinished.AddUniqueDynamic(this, &ThisClass::OnSequencerFinished);
```


With `bOverrideInstanceData` true `ALevelSequenceActor::GetInstanceData()` (from the interface `IMovieScenePlaybackClient`) will return `DefaultInstanceData`.


The transform origin is used by `FGatherTransformOrigin::Run`, which uses `IMovieSceneTransformOrigin` with `NativeGetTransformOrigin`.

The `IMovieScenePlaybackClient` is taken from `FSharedPlaybackState::FindCapability`.
The capability is added in `IMovieScenePlayer::InitializeRootInstance` with `FSharedPlaybackState::AddCapabilityRaw`.

For the `UMovieSceneSequencePlayer`, `IMovieScenePlayer::GetPlaybackClient` returns a cached `PlaybackClient` which is set with `UMovieSceneSequencePlayer::SetPlaybackClient` by `ALevelSequenceActor::PostInitProperties`.

`ALevelSequenceActor` implements the `IMovieScenePlaybackClient` interface.
Same for `UActorSequenceComponent`.
