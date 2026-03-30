
This is done in editor with a menu entry created in `FLevelSequenceEditorActorBinding::BuildSequencerAddMenu`.

Sadly this is hardcoded to `ULevelSequence` as we can see in `FLevelSequenceEditorActorBinding::SupportsSequence`.
You can copy the class and register your own the your editor module to mimic this behavior.

This `FLevelSequenceEditorActorBinding` instance is created in `FLevelSequenceEditorModule::OnCreateActorBinding`, which is a callback of `ISequencerModule::RegisterEditorObjectBinding`.

`FSequencer::BuildAddObjectBindingsMenu` is what filters what object bindings will be displayed in the menu each time it is opened.

So, once you added an actor, `FSequencer::AddActors` is called.
This will also call `FSequencerUtilities::AddActors`.

Once the binding and a lot of stuff has been done, `ISequencer::OnActorAddedToSequencer` is called, which is bound to `FLevelSequenceEditorToolkit::HandleActorAddedToSequencer` in `FLevelSequenceEditorToolkit::Initialize` (`FLevelSequenceEditorToolkit` is crated in `UAssetDefinition_LevelSequence`). 
