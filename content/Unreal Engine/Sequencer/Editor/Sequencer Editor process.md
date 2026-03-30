
When you open a Level Sequence asset `UAssetDefinition_LevelSequence` is used.
Inside `FLevelSequenceEditorToolkit::Initialize` `FSequencerModule::CreateSequencer` is called. Which eventually calls `FSequencer::InitSequencer`.

Important params are passed to `FSequencer::InitSequencer` like callbacks for object bindings (check [[Sequencer Editor Add Actor Track]] for more details on what is happening when you want/add an actor binding track).
