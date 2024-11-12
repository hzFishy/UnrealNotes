Useful links:
- [Cedric's notes on Session Management](https://cedric-neukirchen.net/docs/category/session-management/)
- [Travelling In Multiplayer - Official Docs](https://dev.epicgames.com/documentation/en-us/unreal-engine/travelling-in-multiplayer-in-unreal-engine?application_version=5.3)
- [Session lifecycle](https://docs.redpoint.games/eos-online-subsystem/docs/sessions_lifecycle)

# Online Subsystem

> [!warning]
> Sessions are copied from the server to the client (NOT REPLICATED)
This means that you can still have "valid" session data from a search after a server destroyed the session.

## Create Session
Creates a session using a Name and a Owner (Player Controller Net Id)

## Start Session
Triggers the session to be in `InProgress` state.

## Update Session
Allow to edit session settings.

## Destroy Session
Used by client to leave, or by host on server to destroy a session completely.
Destroying a Session has to happen on both server and clients when they leave.

## Find Sessions
When you call `FindSessions` on the Online Subsystem, the search results array will be filled by `OnValidResponsePacketReceived` if the conditions are true.
The data set in `Session` is done in `ReadSessionFromPacket`

## Debugging
- `EnableCheats true`
- `DumpOnlineSessionState`

# Player Controller
When a Player Controller gets destroyed (`Destroyed`) its calls `Logout` on gamemode, which calls `NotifyLogout` on gamesession, that calls `UnregisterPlayer`, and if it finds a session, it calls `UnregisterPlayer` on that session.
