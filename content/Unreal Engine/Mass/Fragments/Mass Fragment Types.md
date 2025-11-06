Also see [[Mass Fragment Tags]].

# Base types
Base struct types to inherit from.

## `FMassFragment`

## `FMassConstSharedFragment`

## `FMassChunkFragment`

## `FMassSharedFragment`


# Core

## `FTransformFragment`
Simply holds a transform variable.

## `FObjectWrapperFragment`
This is a common type for all the wrappers pointing at UObjects used to copy data from them or set data based on Mass simulation.


# Agent

## `FAgentRadiusFragment`
Simply holds a radius variable.

## `FMassAvoidanceEntitiesToIgnoreFragment`
Usually optional, entities handles to ignore.


# Movement

## `FMassVelocityFragment`
This represents the actual physical velocity of the mass entity in the world. For agents with an actor representation, this is the velocity of the movement component.

## `FMassDesiredMovementFragment`
This is the output of all processors that intend to affect movement.
It is the input to the movement system (e.g. mover, animation etc.)

## `FMassForceFragment`
Accumulator for steering / avoidance forces to apply to the desired velocity.

## `FMassMovementParameters`
Parameters describing how this mass agent should move.

## `FMassCodeDrivenMovementTag`
The presence of this tag indicates that this mass agent's velocity should be controlled by the `FMassDesiredMovementFragment`.
For code driven displacement, we want the desired velocity to affect the velocity directly, which is then applied to the character mover.
For e.g. Root Motion driven displacement, we just need to pipe the DesiredVelocity to the animation system and let it do the test.

# Navigation

## `FMassNavigationEdgesFragment`
Contains edges, filled by `UMassNavMeshNavigationBoundaryProcessor` and used by other processors.

## `FMassMovingAvoidanceParameters`

## `FMassNavMeshCachedPathFragment`
Nav mesh query result.

## `FMassNavMeshShortPathFragment`
Partial data of a nav mesh query result.

# LOD
## `FMassSimulationLODFragment`


# Miscs
## `FMassSimulationVariableTickFragment`
