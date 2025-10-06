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
See also [[State Tree]] and [[State Tree Schemas]]

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
See also `Navigation` folder and [[AI Navigation Types]].

# Perception
- See `Perception` folder and [[AI Perception Types]].
- See `AIPerceptionTypes.h`

# Env Query

## Core
### `UEnvQueryManager`
Created in `UAISystem::PostInitProperties`.
