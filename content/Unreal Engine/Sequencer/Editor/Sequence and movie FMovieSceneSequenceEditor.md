
There are a few editor `FMovieSceneSequenceEditor` derived classes.
- `FMovieSceneSequenceEditor_ActorSequence`
- `FMovieSceneSequenceEditor_WidgetAnimation`
- `FMovieSceneSequenceEditor_TemplateSequence`
- `FMovieSceneSequenceEditor_LevelSequence`

They are all registered with `ISequencerModule::RegisterSequenceEditor`.
Engine example:
```c++
ISequencerModule& SequencerModule = FModuleManager::LoadModuleChecked<ISequencerModule>("Sequencer");
SequenceEditorHandle = SequencerModule.RegisterSequenceEditor(ULevelSequence::StaticClass(), MakeUnique<FMovieSceneSequenceEditor_LevelSequence>());
```

The passed pair of editor and class is stored in `SequenceEditors` which is iterated in `ISequencerModule::FindSequenceEditor`.

The function was made in a way that allows you to add different custom `FMovieSceneSequenceEditor` for an sequence asset type.

