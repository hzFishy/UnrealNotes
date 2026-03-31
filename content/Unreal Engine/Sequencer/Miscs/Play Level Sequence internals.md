
Here is a rough dump on what happens when starting a sequence player.

`UMovieSceneSequencePlayer::Play` -> `UMovieSceneSequencePlayer::PlayInternal` -> `UMovieSceneSequencePlayer::StartTimeControllerAndBroadcastPlayState` -> `UMovieSceneSequencePlayer::UpdateMovieSceneInstance` -> `FMovieSceneEntitySystemRunner::Flush` -> `UMovieSceneTrackInstanceSystem::EvaluateAllInstances` -> `UMovieSceneCameraCutTrackInstance::OnAnimate`

Inside `UMovieSceneCameraCutTrackInstance::OnAnimate` `FCameraCutAnimator::AnimateBlendedCameraCut` is called, which calls `FCameraCutAnimator::FindBoundObject` to get the camera actor.

The `FSharedPlaybackState` state is what holds the binding object cache via its `FPlaybackCapabilities Capabilities`.

