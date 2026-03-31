
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
N/A.

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

## Bindings

### `UMovieSceneCustomBinding`
A custom binding. Allows users to define their own binding resolution types, including dynamic "Replaceable" bindings with previews in editor, as well as Spawnable types.
It has many child classes.

### `UMovieSceneReplaceableBindingBase`
The base class for custom replaceable bindings.
Check source for detailed description.

## Miscs

### `ACineCameraActor`
A CineCameraActor is a CameraActor specialized to work like a cinematic camera.
In the editor it is created with `FSequencerUtilities::CreateCamera`.
You can change how the default "possessing" of the new camera is done with `OnCameraAddedToSequencer` delegate on `ISequencer`, which is handled in `MovieSceneToolHelpers::NewCameraAdded` -> `MovieSceneToolHelpers::LockCameraActorToViewport`. This eventually leads to `UCineCameraComponent::GetCameraView` being called.

### `UCineCameraComponent`
A specialized version of a camera component, geared toward cinematic usage.

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

### `FSubTrackEditor`
Tools for subsequences.
`FSubTrackEditor::BuildAddTrackMenu` is what builds the menu.

# Tracks
## `UMovieScene3DTransformTrack`
Handles manipulation of component transforms in a movie scene.

## `UMovieSceneCommonAnimationTrack`
N/A.

### `UMovieSceneSkeletalAnimationTrack`
Handles animation of skeletal mesh actors.

## `UMovieSceneSubTrack`
A track that holds sub-sequences within a larger sequence.

### `UMovieSceneCinematicShotTrack`
A track that holds consecutive sub sequences.

### `UTemplateSequenceTrack`
N/A.

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

## Miscs
### `IMovieSceneEntityProvider`
Interface to be added to UMovieSceneSection types when they contain entity data.
