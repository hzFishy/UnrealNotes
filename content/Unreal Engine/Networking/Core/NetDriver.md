# Resources
- See `NetDriver.h`
- See also [[NetConnection]]
- See also [[Channel]]
# About

> [!Info] More details in `NetDriver.h`

`UNetDriver`s are responsible for managing sets of `UNetConnection`s, and data that can be shared between them.

Under normal circumstances, there will exist only a single NetDriver (created on Client and Server) for "standard" game traffic and connections.

The **Server NetDriver** will maintain a list of NetConnections, each representing a player that is in the game. It is responsible for replicating Actor Data.
**Client NetDrivers** will have a single NetConnection representing the connection to the Server.

# Startup

> [!Info] More details in `NetDriver.h`

Whenever a server Loads a map (via `UEngine::LoadMap`), we will make a call into `UWorld::Listen`.
That code is responsible for creating the main Game Net Driver, parsing out settings, and calling `UNetDriver::InitListen`.  
And ultimately, that code will be responsible for figuring out what how exactly we listen for client connections. Once the server is listening, it's ready to start accepting client connections.

Whenever a client wants to Join a server, they will first establish a new `UPendingNetGame` in `UEngine::Browse` with the server's IP. `UPendingNetGame::Initialize` and `UPendingNetGame::InitNetDriver` are responsible for initializing settings and setting up the NetDriver respectively.  
Clients will immediately setup a `UNetConnection` for the server as a part of this initialization, and will start sending data to the server on that connection, initiating the handshaking process.

On both Clients and Server, `UNetDriver::TickDispatch` is typically responsible for receiving network data.

After the `UNetDriver` and `UNetConnection` have completed their handshaking process on Client and Server,  `UPendingNetGame::SendInitialJoin` will be called on the Client to kick off game level handshaking (this is where `AGameModeBase::PreLogin`,  `AGameModeBase::GameWelcomePlayer`, .... will be called).

For more details about handling connections, disconnections and more see `NetDriver.h`.
# Data Transmission

> [!Info] More details in `NetDriver.h`

`UNetDriver` and `UNetConnection` work with Packets and Bunches.

**Packets** are blobs of data that are sent between pairs of `NetConnection`s on Host and Client.  
Packets consist of meta data about the packet (such as header information and acknowledgments), and Bunches.

**Bunches** are blobs of data that are sent between pairs of Channels on Host and Client.
When a Connection receives a Packet, that packet will be disassembled into individual bunches.  
Those bunches are then passed along to individual Channels to be processed further.  
A Packet may contain no bunches, a single bunch, or multiple bunches.  
Because size limits for bunches may be larger than the size limits of a single packet, UE supports the notion of partial bunches.

See also [[Replication Flow]].
For more information about handling data loss and how reliable RPCs are handled, see `NetDriver.h`.
