
When using the `Can Blend` feature of a camera cut, it isn't lerping a real scene component between two poses, but using the blending features of the Player Camera Manager.

With blending enabled, `UMovieSceneCameraCutTrackInstance::OnAnimate` will call `FCameraCutAnimator::AnimateBlendedCameraCut`.

This will eventually call `FCameraCutGameHandler::SetCameraCut` which calls `APlayerCameraManager::SetViewTarget`.

In the transition params, `bLockPreviousCamera` is copied to `bLockOutgoing`.

At the end of a sequence `UMovieSceneTrackInstance::Destroy` -> `FPreAnimatedCameraCutTraits::RestorePreAnimatedValue` will be called.
This will call `APlayerCameraManager::SetViewTarget` on the cached `LastViewTarget` (see `FPreAnimatedCameraCutState`).

