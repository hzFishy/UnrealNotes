
The CMC is placed in [[Networking]] because most of the important parts and difficulties occurs when you need replication and prediction.


**Resources**
- [How the CMC works under the hood](https://www.youtube.com/watch?v=urkLwpnAjO0&list=PLXJlkahwiwPmeABEhjwIALvxRSZkzoQpk)
- [UE Custom CMC Network Data](https://docs.google.com/document/d/1UO6Ww6Lfpti3YElVdo9uioTUtQJQ9CoSLvd9kF8hvJo/edit?usp=sharing)
- [Move Containers and combining moves](https://github.com/Vaei/PredictedMovement/wiki/Move-Containers)
- [Implementing predicted sprinting](https://www.youtube.com/watch?v=17D4SzewYZ0&list=PLXJlkahwiwPmeABEhjwIALvxRSZkzoQpk&index=3)
- [CMC Climbing mechanic](https://www.vitorcantao.com/post/climbing-system/)
- [Turn In Place plugin](https://github.com/Vaei/TurnInPlace)

**Optimizing**
- [Character Movement Component Optimizations](https://dev.epicgames.com/community/learning/knowledge-base/mo9O/unreal-engine-character-movement-optimizations)


# Partial Breakdown

When walking, when you move (or tick?) CMC will eventually run the following:
- `StartNewPhysics` calls `PhysWalking` (only call of `PhysWalking`)
	- `PhysWalking` calls `MoveAlongFloor`
		- `MoveAlongFloor` calls `SafeMoveUpdatedComponent`
			- `SafeMoveUpdatedComponent` will run checks and eventually call `MoveUpdatedComponentImpl`, which calls `MoveComponent` on `UpdatedComponent` (by default it is the root, which is the capsule component)
		- Then, here it checks if we are stuck or not. And if its a another ramp (using Normal hit) or wall/barrier.


# Debug
You can visualize basic CMC info using `p.VisualizeMovement 1`

# Client Auth
*Thanks to Jambax*
CMC is server authoritative.
to enable client auth see `bServerAcceptClientAuthoritativePosition` and `bIgnoreClientMovementErrorChecksAndCorrection`.
There is also `AGameNetworkManager`->`ClientAuthorativePosition`.


# Miscs

# Jitter movement when object is attached to character
[In this example](https://discord.com/channels/187217643009212416/221799385611239424/1345390184052555837) the character view jitters because of the interpolation between listen server and clients.
The character jittering is placed on a moving actor (used as base). This moving actor is attached to the other character (that's controlling the moving actor).
The solution is to attach the moving actor on the character mesh, since this is one of the smoothest component of the character for other clients (as the autonomous proxy).

## Client to listen server animation issues
It seems that the listen server can see client character animations lag, this is because by default clients send data to the server at a high frequency (because clients may have a high framerate).
To fix this the idea is to cap the max update frequency.

All of the following isn't required.
*Thanks to Kaos*

```ini title="DefaultGame.ini"
[/Script/Engine.GameNetworkManager]
TotalNetBandwidth=960000
MaxDynamicBandwidth=120000
MinDynamicBandwidth=45000
ClientAuthorativePosition=false
ClientErrorUpdateRateLimit=0.015f
MAXCLIENTUPDATEINTERVAL=0.25f
MAXPOSITIONERRORSQUARED=6.f
MaxClientForcedUpdateDuration=1.f
ServerForcedUpdateHitchThreshold=0.150f
ServerForcedUpdateHitchCooldown=0.100f
MaxMoveDeltaTime=0.125f
ClientNetSendMoveThrottleOverPlayerCount=3
ClientNetSendMoveDeltaTime=0.0166f
ClientNetSendMoveDeltaTimeThrottled=0.0222f
ClientNetSendMoveDeltaTimeStationary=0.0166f
ClientNetSendMoveThrottleAtNetSpeed=60000
bMovementTimeDiscrepancyDetection=false
bMovementTimeDiscrepancyResolution=false
MovementTimeDiscrepancyMaxTimeMargin=0.25f
MovementTimeDiscrepancyMinTimeMargin=-0.25f
MovementTimeDiscrepancyResolutionRate=1.0f
MovementTimeDiscrepancyDriftAllowance=0.05f
bMovementTimeDiscrepancyForceCorrectionsDuringResolution=true
ClientNetCamUpdateDeltaTime=0.066f
BadPingThreshold=200
SeverePingThreshold=500
BadPacketLossThreshold=0.05
SeverePacketLossThreshold=0.15

[/Script/Engine.Player]
ConfiguredInternetSpeed=60000
ConfiguredLanSpeed=60000

[/Script/SteamSockets.SteamSocketsNetDriver]
NetConnectionClassName=/Script/SteamSockets.SteamSocketsNetConnection
ReplicationDriverClassName="/Script/TwinStick.RTS_ReplicationGraph"
ConnectionTimeout=80.0
bNeverApplyNetworkEmulationSettings=true
InitialConnectTimeout=120.0
NetServerMaxTickRate=60
bClampListenServerTickRate=true
MaxNetTickRate=30
KeepAliveTime=0.2
MaxClientRate=120000
MaxInternetClientRate=120000
RelevantTimeout=5.0
SpawnPrioritySeconds=2.0
ServerTravelPause=4.0
```


