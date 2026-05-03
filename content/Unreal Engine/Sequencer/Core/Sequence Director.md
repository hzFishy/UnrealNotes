
The director instance is created from `UMovieSceneSequence::CreateDirectorInstance`.

The base implementation doesn't do anything, the real creation process is done in the overrides.

For `UActorSequence` it returns `ULevel::GetLevelScriptActor` which is an `ALevelScriptActor`.
For `UWidgetAnimation` it returns the widget context which is an `UUserWidget`.
For `ULevelSequence` it returns a new `ULevelSequenceDirector`, it uses a `DirectorClass`.

`UMovieSceneSequence::CreateDirectorInstance` is only called in `FSequenceDirectorPlaybackCapability::GetOrCreateDirectorInstance`.
As far as I tested in 5.6 the `UObject` director instance isn't created/queried every time when playing a sequence.
For example it might only be queried (and created if not already) at runtime *while* playing a sequence if an event key is processed (since the callback is a function member of the director instance).

You can get the director object from the sequence/move player.
Here is an example where we get the director object of a `ULevelSequencePlayer`, it is using `MovieSceneSequenceID::Root` since the sequence doesn't implement any subsequences.
```c++
auto SharedPlaybackState = SequencePlayer->GetSharedPlaybackState();  
auto* DirectorCapability = SharedPlaybackState->FindCapability<UE::MovieScene::FSequenceDirectorPlaybackCapability>();  
auto* DirectorObject = DirectorCapability->GetOrCreateDirectorInstance(SharedPlaybackState, MovieSceneSequenceID::Root);
```

> [!Info]
> From the few engine usages of `AddCapability<FSequenceDirectorPlaybackCapability>()` it seems safe to add yourself the capability if it doesn't exist.

They seem to be stored in a `FDirectorInstanceCache` in a `DirectorInstances` TSortedMap.

The editor "Open director blueprint" calls `FMovieSceneSequenceEditor::GetOrCreateDirectorBlueprint`.

For a level sequence this runs `FMovieSceneSequenceEditor_LevelSequence::CreateBlueprintForSequence` if it doesn't already exist (Hardcoded to `ULevelSequenceDirector::StaticClass()`).
This then calls `ULevelSequence::SetDirectorBlueprint` which sets the `DirectorClass` and `DirectorBlueprint`.

This means that if you want to change the class used in editor you cannot simply overwrite `DirectorClass` in your subclass.

Check [[Sequence and movie FMovieSceneSequenceEditor]] if you want to overwrite the director class.
