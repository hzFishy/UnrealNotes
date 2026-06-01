
See also [[Navigation Data]].

## Core

### `UNavigationSystemBase`


### `UNavigationSystemV1`


### `UNavigationObjectRepository`
World subsystem dedicated to store different types of navigation related elements that the NavigationSystem needs to access.

### `UNavLocalGridManager`
Manager for local navigation grids.
Builds non overlapping grid from multiple sources, that can be used later for pathfinding.  
Check also: `UGridPathFollowingComponent`, `FNavLocalGridData`.

Created in `UAISystem::PostInitProperties`.

### `UNavigationSystemConfig`
Each world settings can hold a `UNavigationSystemConfig`.

## Geometry

### `FNavigableGeometryExport`
Base class.

### `FRecastGeometryExport`
Class that handles geometry exporting for Recast navmesh generation.

## Collision

### `FNavigationRelevantData`
This holds a `FCompositeNavModifier` which holds an array of `Areas` (`FAreaNavModifier`), `SimpleLinks` (`FSimpleLinkNavModifier`), `CustomLinks` (`FCustomLinkNavModifier`) and some other less important parameters.

### `UNavCollisionBase`


### `UNavCollision`


### `FNavCollisionConvex`


## Nav Element

### `FNavigationElement`
Structure registered in the navigation system that holds the required properties and delegates to gather navigation data (navigable geometry, NavArea modifiers, NavLinks, etc.) and be stored in the navigation octree. 
It represents a single element spatially located in a defined area in the level.
That element can be created to represent two use cases:  
1. Single UObject representing the navigation element constructed from a UObject raw or weak pointer  
2. Single UObject managing multiple non-UObject navigation elements constructed from a UObject raw or weak pointer and using the optional constructor parameter to identity a unique sub-element.

It has a `FNavigationDataExport NavigationDataExportDelegate` which is what runs `GetNavigationData` on actor components implementing `INavRelevantInterface`.

### `FNavigationElementHandle`
Structure used to identify a unique navigation element registered in the Navigation system.
The handle can be created to represent two use cases:  
1. Single UObject representing the navigation element constructed from a UObject raw or weak pointer  
2. Single UObject managing multiple non-UObject navigation elements constructed from a UObject raw or weak pointer and using the optional constructor parameter to identity a unique sub-element.


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
