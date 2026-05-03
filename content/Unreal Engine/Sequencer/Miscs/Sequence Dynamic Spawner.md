
See `FMovieSceneSpawnRegister` and `FLevelSequenceSpawnRegister` which are capabilities.

Once a sequence is finished `FSequenceInstance::Finish` is called.
if the sequence is a root sequence `FMovieSceneSpawnRegister` capability will be queried.
If it exist `FMovieSceneSpawnRegister::ForgetExternallyOwnedSpawnedObjects` and `FMovieSceneSpawnRegister::CleanUp` are called.

This will process to destroy the object that were spawned and that should be destroyed (usually the objects that are marked as owned by the sequence that spawned them). See `FMovieSceneSpawnRegister::DestroySpawnedObject`.

The real destruction code should be in `UMovieSceneSpawnableBindingBase` derived classes using the `DestroySpawnedObjectInternal` pure virtual.
