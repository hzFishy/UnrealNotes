
All `UMovieSceneSequencePlayer` have a `TickManager` which is an `UMovieSceneSequenceTickManager` (see `UMovieSceneSequencePlayer::InitializeForTick`).

The ticking is done in `UMovieSceneSequenceTickManager::TickSequenceActors`.
It eventually calls `TickFromSequenceTickManager` for `IMovieSceneSequenceTickManagerClient` which by default is implemented as `UMovieSceneSequencePlayer::TickFromSequenceTickManager`. This calls `UMovieSceneSequencePlayer::UpdateAsync` -> `UMovieSceneSequencePlayer::Update` -> `UMovieSceneSequencePlayer::UpdateTimeCursorPosition`

`TickableClients` are added with `UMovieSceneSequenceTickManager::RegisterTickClient`.
Which is called from `ALevelSequenceActor::SetSequence` -> `UMovieSceneSequencePlayer::Initialize`.
