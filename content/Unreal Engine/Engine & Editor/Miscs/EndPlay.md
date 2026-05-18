
Endplay is called on Actors and Actor Components for various reasons.

## App Exit/Quit
It all starts from `UEngine::PreExit`, then (we are talking about the world `UGameEngine` here) it will iterator all worlds inside `WorldList`, and call various "end" functions.

In order of execution, for each world:
- World `BeginTearingDown`
- `ShutdownWorldNetDriver`
- `FlushLevelStreaming`
- `RouteEndPlay` on all actors
- Game Instance `Shutdown`
- `CleanupWorld`

