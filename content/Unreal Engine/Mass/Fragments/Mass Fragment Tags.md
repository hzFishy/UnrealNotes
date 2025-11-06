Also see [[Mass Fragment Types]].

# Base types
## `FMassTag`

# Movement

## `FMassInLookAtTargetGridTag`
Tag to tell if the entity is in the LookAt target grid

## `FMassCodeDrivenMovementTag`
The presence of this tag indicates that this mass agent's velocity should be controlled by the `FMassDesiredMovementFragment`.
For code driven displacement, we want the desired velocity to affect the velocity directly, which is then applied to the character mover.
For e.g. Root Motion driven displacement, we just need to pipe the DesiredVelocity to the animation system and let it do the test.

## `FMassSimpleMovementTag`

# Agent/Nav
## `FMassInNavigationObstacleGridTag`
Tag to tell if the entity is in the navigation obstacle grid.

# Visualizer/Representation
## `FMassDistanceLODProcessorTag`
Tag required by Distance LOD Processor to update LOD information. Removing the tag allows to support temporary disabling of processing for individual entities.

## `FMassStaticRepresentationTag`

## `FMassVisualizationProcessorTag`

## `FMassStationaryISMSwitcherProcessorTag`
Tag required by `UMassStationaryISMSwitcherProcessor` to process given archetype. Removing the tag allows to support temporary disabling of processing for individual entities of given archetype.

## `FMassVisualizationLODProcessorTag`
Tag required by Visualization LOD Processor to update LOD information. Removing the tag allows to support temporary disabling of processing for individual entities.

# LOD

## `FMassHighLODTag`

## `FMassMediumLODTag`

## `FMassLowLODTag`

## `FMassOffLODTag`

## `FMassCollectDistanceLODViewerInfoTag`
Tag required by Visualization LOD Processor to update LOD information. Removing the tag allows to support temporary disabling of processing for individual entities.

## `FMassCollectLODViewerInfoTag`

## `FMassVisibilityCanBeSeenTag`

## `FMassVisibilityCulledByDistanceTag`

## `FMassVisibilityCulledByFrustumTag`

# State Tree
## `FMassStateTreeActivatedTag`
Special tag to know if the state tree has been activated.

# Smart Objects
## `FMassInActiveSmartObjectsRangeTag`
Mass Tag applied on entities with FFSmartObjectRegistrationFragment that need to create smart objects.

## `FMassSmartObjectCompletedRequestTag`
Special tag to mark processed requests.


# Networking
## `FMassInReplicationGridTag`
Component Tag to tell if the entity is in the replication grid.

# Translator
## `FMassCapsuleTransformCopyToActorTag`

## `FMassCapsuleTransformCopyToMassTag`

## `FMassCharacterMovementCopyToActorTag`

## `FMassCharacterMovementCopyToMassTag`

## `FMassCharacterOrientationCopyToActorTag`

## `FMassCharacterOrientationCopyToMassTag`

## `FMassSceneComponentLocationCopyToActorTag`

## `FMassSceneComponentLocationCopyToMassTag`


# Debug
## `FMassDebuggableTag`
