- See `Engine\Source\Runtime\AIModule`
- See `AITypes.h`

# Core

## `AAIController`
`AIController` is the base class of controllers for AI-controlled Pawns.  
`AIController`s manage the artificial intelligence for the pawns they control.  

## `UAIHotSpotManager`
Created in `UAISystem::PostInitProperties`.

## `UAISystemBase`


## `UAISystem`


## `UAISubsystem`


# Logic

## Messages
### `FAIMessage`


### `FAIMessageObserver`


## Brain
### `UBrainComponent`


## Behavior Tree
### `UBehaviorTreeManager`
Created in `UAISystem::PostInitProperties`.

### `UBehaviorTreeComponent`
Component used to run a behavior tree.

## State Tree
### `UStateTreeComponent`
Component used to run a state tree.

### `UStateTreeAIComponent`
State tree component designed to be run on an `AIController`.
Place it on your AI Controller.

### `IStateTreeSchemaProvider`
Implementing this interface allows derived class to override the schema used to filter valid state trees for a `FStateTreeReference`.
The state tree reference property needs to be marked with `SchemaCanBeOverriden` metatag.

# Resources
## `IAIResourceInterface`


# Navigation
See also `Navigation` folder.

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

# Perception
- See also `Perception` folder.
- See `AIPerceptionTypes.h`

## Core
### `UAIPerceptionSystem`
AI Subsystem managing AI Perception through registered AI Senses between Listeners and Stimuli Sources.

Created in `UAISystem::PostInitProperties`.

This subsystem holds a list of `Senses`.
Inside `UAIPerceptionSystem::Tick` it calls `UAISense::Tick` for each `Senses`.
This function calls `UAISense::Update`, which does something different depending on the sight.

### `UAIPerceptionStimuliSourceComponent`
Gives owning actor a way to auto-register as perception system's sense stimuli source.

### `UPawnSensingComponent`
**Deprecated since 5.5.**

### `IAIPerceptionListenerInterface`


## Senses - Base
See `UAIPerceptionSystem` for more information on how they are used.

### `UAISense`
Base class of all sense classes.

### `UAISenseEvent`


## Senses - Sight
### `UAISense_Sight`
Inside `UAISense_Sight::Update`, `UAISense_Sight::ComputeVisibility` is called, which calls `FAISystem::CheckIsTargetInSightCone`.

### `IAISightTargetInterface`


### `FAISightQuery`


## Senses - Blueprint
### `UAISense_Blueprint`


## Senses - Damage
### `UAISense_Damage`


## Senses - Hearing
### `UAISense_Hearing`


## Senses - Prediction
### `UAISense_Prediction`


## Senses - Team
### `UAISense_Team`


## Senses - Touch
### `UAISense_Touch`


# Env Query

## Core
### `UEnvQueryManager`
Created in `UAISystem::PostInitProperties`.
