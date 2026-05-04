
# Core

## Runtime

### `UMovieSceneSequenceTickManager`
Global (one per-UWorld) manager object that manages ticking and updating any and all Sequencer-based evaluations for the current frame, before any other actors are ticked.

Ticking clients are registered based on their desired tick interval, and grouped together with other clients that tick with the same interval (based on Sequencer.TickIntervalGroupingResolutionMs). 

Sequencer data is shared between all instances within the same group, allowing them to blend together. Clients ticking at different intervals do not support blending with each other.

### `ALevelSequenceActor`
Actor responsible for controlling a specific level sequence in the world.
Also check `AReplicatedLevelSequenceActor`.

### `IMovieScenePlaybackClient`
Movie scene binding overrides interface.
`ALevelSequenceActor` and `UActorSequenceComponent` implements it.

### `IMovieSceneBindingOwnerInterface`
N/A.

### `UMovieSceneSequence`
Abstract base class for movie scene animations (C++ version).
The very base parent of it is `UMovieSceneSignedObject`.

### `UMovieSceneSequencePlayer`
Abstract class that provides consistent player behavior for various animation players.

### `IMovieSceneSequenceTickManagerClient`
Interface for objects that are to be ticked by the tick manager.

### `ULevelSequence`
Movie scene animation for Actors.

### `ULevelSequencePlayer`
`ULevelSequencePlayer` is used to actually "play" an level sequence asset at runtime.
This class keeps track of playback state and provides functions for manipulating an level sequence while its playing.

### `ISequencer`
Interface for sequencers.
Note: As for 5.6 this is only used in `FSequencer`.

### `IMovieScenePlayer`
Interface for movie scene players.
Provides information for playback of a movie scene

### `UMovieScene`
Implements a movie scene asset.

### `UMovieSceneDecorationContainerObject`
N/A.

### `UMovieSceneClock`
N/A.

### `FSharedPlaybackState`
A structure that stores playback state for an entire sequence hierarchy.
As of 5.6 this struct has only 2 constructors, one using an `UMovieSceneEntitySystemLinker` (usages only shows result in MovieScenePreAnimatedStateTests.cpp) and the second one a `UMovieSceneSequence` root sequence a the `FSharedPlaybackStateCreateParams` struct.

### `FSharedPlaybackStateCreateParams`
Parameter structure for initializing a new shared playback state.
The `PlaybackContext` seems to always be an `UWorld` from the engine write access, but it is unclear if its mandatory (feels like its just a "World Context" object; so as long as `GetWorld` on it is correctly implemented you should be fine).

### `FSequenceInstance`
A sequence instance represents a specific instance of a currently playing sequence, either as a top-level sequence in an `IMovieScenePlayer`, or as a sub sequence.
Any given sequence asset may have any number of instances created for it at any given time depending on how many times it is referenced by playing sequences

Note: `MovieSceneHelpers::CreateTransientSharedPlaybackState` can be used to create temporary playback states (Keep in mind this creates a very basic and "fake" playback state so it only holds a `FMovieSceneEvaluationState` capability).

## Bindings

### `UMovieSceneCustomBinding`
A custom binding. Allows users to define their own binding resolution types, including dynamic "Replaceable" bindings with previews in editor, as well as Spawnable types.
It has many child classes.

### `UMovieSceneReplaceableBindingBase`
The base class for custom replaceable bindings.
Check source for detailed description.

### `UMovieSceneBindingOverrides`
A one-to-many definition of movie scene object binding IDs to overridden objects that should be bound to that binding.

### `FMovieSceneBindingOverrideData`

## Miscs

### `UMovieSceneSequenceExtensions`
Contains many interesting helper functions exposed to blueprints.

### `MovieSceneHelpers`
Contains many interesting c++ only helper functions.

### `ACineCameraActor`
A CineCameraActor is a CameraActor specialized to work like a cinematic camera.
In the editor it is created with `FSequencerUtilities::CreateCamera`.
You can change how the default "possessing" of the new camera is done with `OnCameraAddedToSequencer` delegate on `ISequencer`, which is handled in `MovieSceneToolHelpers::NewCameraAdded` -> `MovieSceneToolHelpers::LockCameraActorToViewport`. This eventually leads to `UCineCameraComponent::GetCameraView` being called.

### `UCineCameraComponent`
A specialized version of a camera component, geared toward cinematic usage.

### `FEntityBuilder`
Derived from `TEntityBuilder`.

### `TEntityBuilder`
See source code for detailed description.

### `FBuiltInComponentTypes`
Pre-defined built in component types.

## Editor Only

### `ULevelSequenceEditorSettings`
See `TrackSettings` if you want to create automatically tracks for specific added actor types.

### `ISequencerModule`
Used by `FSequencerModule`.

### `ILevelSequenceEditorModule`
Used by `FLevelSequenceEditorModule`.

### `FLevelSequenceEditorToolkit`
Implements an Editor toolkit for level sequences.
Child of `ILevelSequenceEditorToolkit`.

### `ULevelSequenceFactoryNew`
Implements a factory for `ULevelSequence` objects.

### `FSequencer`
Sequencer is the editing tool for MovieScene assets.
Is responsible for the creation of `SSequencer` (see `SequencerWidget`).
It is marked as `final` so it cannot be subclassed.
It is created in `FSequencerModule::CreateSequencer`, there is 2 delegates (`OnPreSequencerInit` and `OnSequencerCreated`) that allows you to inject stuff, check the Register functions for public access .
Note: `InitSequencer` calls `BindCommands`.

### `FSequencerUtilities`
N/A.

### `MovieSceneToolHelpers`
N/A.

### `ISequencerEditorObjectBinding`
Only child is `FLevelSequenceEditorActorBinding`.

### `FObjectBindingTagCache`
Owned by an `FSequencer` instance, this class tracks tags for object bindings, and maintains a reverse lookup map from binding to tag(s) along with other information for the tags such as color tints.
See [[Sequence tags]] for more info.

### `FSubTrackEditor`
Tools for subsequences.
`FSubTrackEditor::BuildAddTrackMenu` is what builds the menu.

# Tracks
## `UMovieScene3DTransformTrack`
Handles manipulation of component transforms in a movie scene.

## `UMovieSceneCommonAnimationTrack`
N/A.

## `UMovieSceneSkeletalAnimationTrack`
Child of `UMovieSceneCommonAnimationTrack`.
Handles animation of skeletal mesh actors.

### `UMovieSceneSkeletalAnimationTrack`
Handles animation of skeletal mesh actors.

## `UMovieSceneSubTrack`
A track that holds sub-sequences within a larger sequence.

### `UMovieSceneCinematicShotTrack`
A track that holds consecutive sub sequences.

### `UTemplateSequenceTrack`
N/A.

## `UMovieSceneEventTrack`
Implements a movie scene track that triggers discrete events during playback.

# Sections

## `UMovieScene3DTransformSection`
A 3D transform section.
See [[Getting Sequencer Tracks and Sections]] for an example on how to get the transform values.

## `UMovieSceneSkeletalAnimationSection`
Movie scene section that control skeletal animation.

## `UMovieSceneSubSection`
Implements a section in sub-sequence tracks.
They are added with `FSubTrackEditor::HandleAddSubSequenceComboButtonMenuEntryExecute` -> `UMovieSceneSubTrack::AddSequenceOnRow`.

### `UMovieSceneCinematicShotSection`
Implements a cinematic shot section.

### `UTemplateSequenceSection`
Defines the section for a template sequence track.

## `UMovieSceneEventSection`
Implements a section in movie scene event tracks.
Seems weird that this exists since `UMovieSceneEventSectionBase` is here to but they don't seem related.

## `UMovieSceneEventSectionBase`
Base class for all event sections. Manages dirtying the section and track on recompilation of the director blueprint.

### `UMovieSceneEventRepeaterSection`
Event section that will trigger its event exactly once, every time it is evaluated.

### `UMovieSceneEventTriggerSection`
Event section that triggers specific timed events.

## Miscs
### `IMovieSceneEntityProvider`
Interface to be added to UMovieSceneSection types when they contain entity data.
