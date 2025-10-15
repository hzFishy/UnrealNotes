
## Server Travel
Use `UWorld::ServerTravel` on server to make a server travel + all the connect clients.
If not absolute, you can set a new game mode by adding the following option to the URL :
`?Game=Full/Path/To/MyGameMode`

## Seamless travel
See [Docs](https://dev.epicgames.com/documentation/en-us/unreal-engine/travelling-in-multiplayer-in-unreal-engine#enablingseamlesstravel).
`bUseSeamlessTravel` must be enabled on the current GM, not the "target" one.
