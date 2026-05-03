
Most capabilities structs seems to be not a derived struct from an interface or base struct.
Check `UE_DECLARE_MOVIESCENE_PLAYBACK_CAPABILITY_API` macro usages for examples of declared types.

# Types

## `IPlaybackCapability`
Interface for playback capabilities that want to be notified of various operations.

### `FSequenceDirectorPlaybackCapability`
Playback capability for sequences that have a director blueprint.

### `FMovieSceneEvaluationState`
Provides runtime evaluation functions with the ability to look up state from the main game environment.

### `FMovieSceneSpawnRegister`
Class responsible for managing spawnables in a movie scene.
Has derived classes, common child is `FLevelSequenceSpawnRegister`.

### `FCameraCutPlaybackCapability`
Playback capability for sequences that can run camera cuts.

### `IEventContextsPlaybackCapability`
Playback capability for controlling how events are triggered.
