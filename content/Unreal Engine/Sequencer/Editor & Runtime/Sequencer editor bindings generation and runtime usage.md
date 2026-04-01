
# Editor 
When you click on "create new endpoint" `FMovieSceneDirectorBlueprintEndpointCustomization::CreateEndpoint` is called.
The callback is bound from a lambda in `FMovieSceneConditionCustomization::FillDirectorBlueprintConditionSubMenu`.

This goes through `FMovieSceneDirectorBlueprintUtils::CreateEndpoint` which calls `FMovieSceneDirectorBlueprintUtils::CreateEventEndpoint` or `FMovieSceneDirectorBlueprintUtils::CreateFunctionEndpoint`.

Note that `FMovieSceneDirectorBlueprintEndpointCustomization` is a base class for
- `FMovieSceneDirectorBlueprintConditionCustomization`
- `FMovieSceneDynamicBindingCustomization`
- `FMovieSceneEventCustomization`

For instance in this example where I am adding an Sequence blueprint Direction endpoint on a Replaceable actor `FMovieSceneDynamicBindingCustomization::OnCreateEndpoint` will eventually call `FMovieSceneDynamicBindingUtils::SetEndpoint`.
This set a weak endpoint EDITOR ONLY `UK2Node` var on a `FMovieSceneDynamicBinding`.

After `FMovieSceneDirectorBlueprintEndpointCustomization::CreateEndpoint` ran `NotifyFinishedChangingProperties` is called which triggers a callback at `ULevelSequenceEditorSubsystem::OnFinishedChangingLocators`. This eventually calls `FSequencer::RecompileDirtyDirectors`.

Via the `FKismetCompilerContext` this will call `UMovieSceneDynamicBindingBlueprintExtension::HandleGenerateFunctionGraphs` which uses `FMovieSceneDynamicBindingUtils::IterateDynamicBindings` to iterate all bindings.

Inside a lambda declared in `HandleGenerateFunctionGraphs`, `FMovieSceneDirectorBlueprintUtils::GenerateEntryPoint` is called which will set the EDITOR ONLY `FName CompiledFunctionName` var on the `FMovieSceneDynamicBinding`.

`TFieldPath<FProperty> ResolveParamsProperty` is also filled if any extra params must be known.

Later in the same function the real runtime `TObjectPtr<UFunction> Function` var is set on the dynamic binding struct using `UClass::FindFunctionByName`.

# Runtime
To check how the endpoint is called at runtime, I played my sequencer in game.
After the same common functions that are called when calling Play on the sequence player (see [[Play Level Sequence internals]]) `FBoundObjectTask::ForEachAllocation` will call `FSharedPlaybackState::FindBoundObjects` -> `FMovieSceneObjectCache::FindBoundObjects` -> `FMovieSceneObjectCache::UpdateBindings` -> `ULevelSequencePlayer::ResolveBoundObjects` -> `FMovieSceneBindingReferences::ResolveBinding` -> `UMovieSceneReplaceableBindingBase::ResolveBinding`.

Its inside `UMovieSceneReplaceableDirectorBlueprintBinding::ResolveRuntimeBindingInternal` -> `FMovieSceneDynamicBindingInvoker::ResolveDynamicBinding` that the function is checked and inside `FMovieSceneDynamicBindingInvoker::InvokeDynamicBinding` that the function is really called with `UObject::ProcessEvent` on the director instance.

In another example I created a child class of `UMovieSceneReplaceableActorBinding_BPBase` to use an endpoint that isn't using the BP sequence director.
The main callstack is the same (all the way to `UMovieSceneReplaceableBindingBase::ResolveBinding`), but `UMovieSceneReplaceableBindingBase::ResolveBinding` was the virtual override.

Note that `FMovieSceneBindingReference` holds a `CustomBinding` var which is a `UMovieSceneCustomBinding`.
This is inside `FMovieSceneBindingReferences`.

Another side note is that `ResolveRuntimeBindingInternal` in `UMovieSceneReplaceableBindingBase` is the actual C++ function that returns the resolved binding result.

There is also a `UMovieSceneBindingOverrides` class.
The `ALevelSequenceActor` has one.
Which is edited by the many binding functions, and ultimately queried with `ALevelSequenceActor::RetrieveBindingOverrides` (an `IMovieScenePlaybackClient` virtual).
