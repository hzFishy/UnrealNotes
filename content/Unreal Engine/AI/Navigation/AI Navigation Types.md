
## Core
### `UNavLocalGridManager`
Manager for local navigation grids.
Builds non overlapping grid from multiple sources, that can be used later for pathfinding.  
Check also: `UGridPathFollowingComponent`, `FNavLocalGridData`.

Created in `UAISystem::PostInitProperties`.

### `UNavigationSystemBase`


## Agent
### `INavAgentInterface`
This is implemented by the Pawn and Controller classes.

### `FNavAgentProperties`
Properties of representation of an 'agent' (or Pawn) used by AI navigation/pathfinding.

## Path
### `UPathFollowingComponent`


### `UCrowdFollowingComponent`


### `UGridPathFollowingComponent`
Path following augmented with local navigation grids.
Keeps track of nearby grids and use them instead of navigation path when agent is inside.
Once outside grid, regular path following is resumed.  This allows creating dynamic navigation obstacles with fully static navigation (e.g. static navmesh), as long as they are minor modifications for path. Not recommended for blocking off entire corridors.
Does not replace proper avoidance for dynamic obstacles!

### `IPathFollowingAgentInterface`

## Movement

### `UNavMovementComponent`
NavMovementComponent defines base functionality for MovementComponents that move any 'agent' that may be involved in AI pathfinding.

### `INavMovementInterface`
Interface for navigation movement - should be implemented on movement objects that control an object directly.

### `FMovementProperties`
Movement capabilities, determining available movement options for Pawns and used by AI for reachability tests.

### `FNavMovementProperties`
Struct to hold properties a user might set for navigation movement.

### `FAIMoveRequest`
Unique struct made on AI move request.
