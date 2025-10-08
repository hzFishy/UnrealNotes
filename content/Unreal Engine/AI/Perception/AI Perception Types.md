

# `UAIPerceptionSystem`

## About
AI Subsystem managing AI Perception through registered AI Senses between Listeners and Stimuli Sources.

Created in `UAISystem::PostInitProperties`.


Inside `UAIPerceptionSystem::Tick` it calls `UAISense::Tick` for each `Senses`.
This function calls `UAISense::Update`, which does something different depending on the sense behavior.

## Senses management
This subsystem holds a list of `Senses`.

### Sense registration
`UAIPerceptionSystem::RegisterSenseClass` is the function which updates the `Senses` array.
This function is only called in `UAIPerceptionComponent::RegisterSenseConfig` and `UAIPerceptionSystem::RegisterSourceForSenseClass` (in case a `UAIPerceptionStimuliSourceComponent` registers with a sense class that wasn't registered yet).

### Stimuli source registration
`UAIPerceptionStimuliSourceComponent` registers themselves as source of stimuli via `UAIPerceptionSystem::RegisterSourceForSenseClass` which calls `UAIPerceptionSystem::RegisterSource`. This adds a unique `FAISenseID` with it's SourceActor in a temporary array called `SourcesToRegister` (entry is of type `FPerceptionSourceRegistration`).

This array is iterated in `UAIPerceptionSystem::Tick` via `UAIPerceptionSystem::PerformSourceRegistration`.
If the source actor is still valid and the sense class was already previously registered and added to the `Senses` array this will call `UAISense::RegisterSource` as well as create a new `FPerceptionStimuliSource` entry in the `RegisteredStimuliSources` array.

## Listener Containers
`ListenerContainer` invalid entries are remove if necessary in `UAIPerceptionSystem::Tick` and `UAIPerceptionSystem::UnregisterListener`.
Entries are added in `UAIPerceptionSystem::UpdateListener` if new.
See usages of `UAIPerceptionSystem::OnNewListener`, `UAIPerceptionSystem::OnListenerUpdate` and `UAIPerceptionSystem::OnListenerRemoved`.


# Components
## `UAIPerceptionComponent`
Placed on AI Controller.
AIPerceptionComponent is used to register as stimuli listener in AIPerceptionSystem and gathers registered stimuli. UpdatePerception is called when component gets new stimuli (batched).

`StimuliToProcess` is updated via `UAIPerceptionComponent::RegisterStimulus`


## `UAIPerceptionStimuliSourceComponent`
Gives owning actor a way to auto-register as perception system's sense stimuli source.
To register the owner actor with the given senses, you must have `bAutoRegisterAsSource` enabled or manually call `RegisterWithPerceptionSystem`.

`RegisterWithPerceptionSystem` will get the unique `UAIPerceptionSystem` for the world through `UAISystem` (see `UAIPerceptionSystem::GetCurrent`).
Then for each senses in the array it will call `UAIPerceptionSystem::RegisterSourceForSenseClass`.

## `UPawnSensingComponent`

> [!Warning] Deprecated since 5.5.

# Interfaces
## `IAIPerceptionListenerInterface`


# Senses
## Senses - Base
See `UAIPerceptionSystem` for more information on how they are used.

### `FAISenseID`
Typedef of `FAINamedID<FAISenseCounter>`.

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


# Stimuli

## `FStimulusToProcess`


# Miscs

## `FPerceptionSourceRegistration`

## `FPerceptionStimuliSource`

## `FPerceptionChannelAllowList`

## `FPerceptionListener`
Should contain only cached information common to all senses. Sense-specific data needs to be stored by senses themselves.

It holds a weak ref to an `UAIPerceptionComponent`.
