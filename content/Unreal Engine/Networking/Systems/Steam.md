
# Resources
- [UE Docs](https://dev.epicgames.com/documentation/en-us/unreal-engine/online-subsystem-steam-interface-in-unreal-engine)


# Steam Sockets Plugin

Steam Sockets is useful if you want "P2P" behavior where player connects to each other with their ports without extra configuration on their side.
This also uses Steam relays, which will help to hide player IPs between each other.

This can be used for listen server and dedicated server P2P.

If you use it you need to change the used NetDriverDefinition: 
`+NetDriverDefinitions=(DefName="GameNetDriver",DriverClassName="SteamSockets.SteamSocketsNetDriver",DriverClassNameFallback="OnlineSubsystemUtils.IpNetDriver")`

You also can't use OpenLevel for sessions, use ServerTravel (with seamless travel).

For reason codes see [this Steam SDK docs page](https://partner.steamgames.com/doc/api/steamnetworkingtypes#ESteamNetConnectionEnd) for a list.
