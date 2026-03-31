Similar to [[Sequencer Editor Add Actor Track]] this is hardcoded to work with `ULevelSequence`.
What is happening is that inside `FSequencer::BindCommands` `CreateCamera` UICommand has a `FIsActionButtonVisible` callback set which hides the button if using VR or not the exact `UlevelSequence`.

You can simply fix this by adding the following pseudo code in your editor module:
```c++
// In module startup
ISequencerModule& SequencerModule = FModuleManager::LoadModuleChecked<ISequencerModule>("Sequencer");
OnSequencerCreatedDelegateHandle = SequencerModule.RegisterOnSequencerCreated(FOnSequencerCreated::FDelegate::CreateStatic(&FMyEditorModule::OnSequencerCreated));

// callback
void FMyEditorModule::OnSequencerCreated(TSharedRef<ISequencer> Sequencer)
{
	TSharedPtr<FUICommandList> CommandList = Sequencer->GetCommandBindings(ESequencerCommandBindings::Sequencer);
	const FSequencerCommands& Commands = FSequencerCommands::Get();
	
	// override action for CreateCamera to remove hardcoded ExactCast check on ULevelSequence
	CommandList->UnmapAction(Commands.CreateCamera);
	CommandList->MapAction(
		Commands.CreateCamera,
		FExecuteAction::CreateLambda([WeakSequencer = Sequencer.ToWeakPtr()]
		{
			ACineCameraActor* OutActor;
			FSequencerUtilities::CreateCamera(WeakSequencer.Pin().ToSharedRef(), true, OutActor); 
		})
	);
}
```

Don't forget to call `UnregisterOnSequencerCreated` on shutdown !
