
It seems that the listen server can see client character animations lag, this is because by default clients send data to the server at a high frequency (because clients may have a high framerate).
To fix this the idea is to cap the max update frequency.

(thanks to Kaos)

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


