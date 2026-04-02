
The director instance is created from `UMovieSceneSequence::CreateDirectorInstance`.

The base implementation doesn't do anything, the real creation process is done in the overrides.

For `UActorSequence` it returns `ULevel::GetLevelScriptActor` which is an `ALevelScriptActor`.
For `UWidgetAnimation` it returns the widget context which is an `UUserWidget`.
For `ULevelSequence` it returns a new `ULevelSequenceDirector`, it uses a `DirectorClass`.

`UMovieSceneSequence::CreateDirectorInstance` is only called in `FSequenceDirectorPlaybackCapability::GetOrCreateDirectorInstance`.

They seem to be stored in a `FDirectorInstanceCache` in a `DirectorInstances` TSortedMap.

The editor "Open director blueprint" calls `FMovieSceneSequenceEditor::GetOrCreateDirectorBlueprint`.

For a level sequence this runs `FMovieSceneSequenceEditor_LevelSequence::CreateBlueprintForSequence` if it doesn't already exist (Hardcoded to `ULevelSequenceDirector::StaticClass()`).
This then calls `ULevelSequence::SetDirectorBlueprint` which sets the `DirectorClass` and `DirectorBlueprint`.

This means that if you want to change the class used in editor you cannot simply overwrite `DirectorClass` in your subclass.

Check [[Sequence and movie FMovieSceneSequenceEditor]] if you want to overwrite the director class.
