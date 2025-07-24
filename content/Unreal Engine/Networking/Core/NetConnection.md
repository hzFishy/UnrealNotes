# Resources
- See `NetDriver.h`
- See also [[NetDriver]]
- See also [[Channel]]

# About
`NetConnection`s represent individual clients that are connected to a game (or more generally, to a `NetDriver`).

End point data isn't directly handled by NetConnections. Instead NetConnections will route data to Channels.
Every NetConnection will have its own set of channels.

For split screens see `UChildConnection`.
