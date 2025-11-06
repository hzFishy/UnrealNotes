
# Agent/Nav

## `UMassNavMeshNavigationBoundaryProcessor`
Fills `FMassNavigationEdgesFragment` Edges.

## `UMassNavMeshPathFollowProcessor`
Processor for updating move target on a navmesh short path.

## `UMassMovingAvoidanceProcessor`
Move using cumulative forces to avoid close agents.

## `UMassStandingAvoidanceProcessor`
Avoidance while standing.

## `UMassNavigationObstacleGridProcessor`
Processor to update obstacle grid.

## `UMassNavigationObstacleRemoverProcessor`
Deinitializer processor to remove avoidance obstacles from the avoidance obstacle grid.

## `UMassNavigationSmoothHeightProcessor`
Updates entities height to move targets position smoothly. 
Does not update Off-LOD entities.

## `UMassOffLODNavigationProcessor`
Updates Off-LOD entities position to move targets position.

# Movement

## `UMassApplyForceProcessor`
Calculate desired movement based on input forces.

## `UMassLookAtProcessor`
Processor to choose and assign LookAt configurations.

## `UMassLookAtRequestDeinitializer`
Deinitializer processing deleted LookAt requests.

## `UMassLookAtRequestInitializer`
Initializer processing new LookAt requests.

## `UMassLookAtTargetGridProcessor`
Processor to maintain a list of LookAt targets in a spatial query structure in the subsystem.

## `UMassLookAtTargetRemoverProcessor`
Deinitializer processor to remove targets from the hash grid.

## `UMassSmoothOrientationProcessor`
Updates agent's orientation based on current movement.

## `UMassSteerToMoveTargetProcessor`
Processor for updating steering towards MoveTarget.

## `UMassMoveTargetFragmentInitializer`
Initializes the move target's location to the agents initial position.

## `UMassSimpleMovementProcessor`

## `UMassRandomVelocityInitializer`

## `UMassApplyMovementProcessor`
Updates entities position based on desired velocity. Only required for agents that have code driven displacement. Not applied on Off-LOD entities.

# Representation/Visualizer
## `UMassRepresentationFragmentDestructor`

## `UMassRepresentationProcessor`

## `UMassVisualizationProcessor`

## `UMassStationaryISMRepresentationFragmentDestructor`
This class is responsible for cleaning up ISM instances visualizing stationary entities.

## `UMassStationaryISMSwitcherProcessor`
This processor's sole responsibility is to process all entities tagged with FMassStaticRepresentationTag and check if they've switched to or away from EMassRepresentationType::StaticMeshInstance; and acordingly add or remove the entity from the appropriate FMassInstancedStaticMeshInfoArrayView.

## `UMassUpdateISMProcessor`

## `UMassVisualizationLODProcessor`


# LOD
## `UMassLODCollectorProcessor`
LOD collector which combines collection of LOD information for both Viewer and Simulation LODing when possible.

## `UMassLODDistanceCollectorProcessor`
LOD Distance collector which combines collection of LOD information for both Viewer and Simulation LODing.
This collector cares only about the entities' distance to LOD viewer location, nothing else. Matches MassDistanceLODProcessor logic which uses the same Calculator LODLogic.

## `UMassSimulationLODProcessor`

## `UMassDistanceLODProcessor`

# State Tree
## `UMassStateTreeActivationProcessor`
Processor to send the activation signal to the state tree which will execute the first tick

## `UMassStateTreeFragmentDestructor`
Processor to stop and uninitialize StateTrees on entities.

## `UMassStateTreeProcessor`
The processor that the UMassStateTreeSubsystem will instantiate for every unique StateTree Mass-requirements.
The user is not expected to instantiate these processors manually, but a project-specific extension can be implemented.
It needs to derive from UMassStateTreeProcessor and set as the value of UMassStateTreeSubsystem.DynamicProcessorClass.

## `UMassDebugStateTreeProcessor`


# Zone Graph
## `UMassZoneGraphAnnotationTagUpdateProcessor`
Processor for update ZoneGraph annotation tags periodically and on lane changed signal.

## `UMassZoneGraphAnnotationTagsInitializer`
Processor for initializing ZoneGraph annotation tags.

## `UMassZoneGraphLaneCacheBoundaryProcessor`
ZoneGraph lane cache boundary processor

## `UMassZoneGraphLocationInitializer`
Processor for initializing nearest location on ZoneGraph.

## `UMassZoneGraphPathFollowProcessor`
Processor for updating move target on ZoneGraph path.

# EQS
## `UMassEnvQueryGeneratorProcessor_MassEntityHandles`
Processor for completing MassEQSSubsystem Requests sent from `UMassEnvQueryGenerator_MassEntityHandles`.

## `UMassEnvQueryProcessorBase`
Processor for completing MassEQSSubsystem Requests sent from `UMassEnvQueryTest_MassEntityTags`.

## `UMassEnvQueryTestProcessor_MassEntityTags`
Processor for completing MassEQSSubsystem Requests sent from `UMassEnvQueryTest_MassEntityTags`.


# Smart Objects
## `UMassSmartObjectCandidatesFinderProcessor`
Processor that builds a list of candidates objects for each users.

## `UMassSmartObjectTimedBehaviorProcessor`
Processor for time based user's behavior that waits x seconds then releases its claim on the object.

## `UMassSmartObjectUserFragmentDeinitializer`
Deinitializer processor to unregister slot invalidation callback when SmartObjectUser fragment gets removed.

## `UMassActiveSmartObjectDeinitializer`
Signals entities with UE::Mass::Signals::SmartObjectActivationChanged when `FMassInActiveSmartObjectsRangeTag` is removed.

## `UMassActiveSmartObjectInitializer`
Signals entities with UE::Mass::Signals::SmartObjectActivationChanged when `FMassInActiveSmartObjectsRangeTag` is added.

## `UMassActiveSmartObjectSignalProcessor`
Signal based processor that creates and destroys the smart object instance associated to an entity based on valid `FSmartObjectRegistration` and `FMassActorInstance` fragments.
The registration is processed on the following events:  
*    UE::Mass::Signals::ActorInstanceHandleChanged  
*    UE::Mass::Signals::SmartObjectActivationChanged  
Also see `FFSmartObjectRegistrationFragment`.

## `UMassActorInstanceHandleDeinitializer`
Signals entities with UE::Mass::Signals::ActorInstanceHandleChanged when `FMassActorInstanceFragment` is removed.

## `UMassActorInstanceHandleInitializer`
Signals entities with UE::Mass::Signals::ActorInstanceHandleChanged when `FMassActorInstanceFragment` is added.

## `UMassSmartObjectDeinitializerBase`
Processor that signals entities with `FSmartObjectRegistration` and `FMassActorInstanceFragment` fragments when the a given tag or fragment is removed from an entity.  
See `FSmartObjectRegistrationFragment` and `FMassActorInstanceFragment`.

## `UMassSmartObjectInitializerBase`
Processor that signals entities with `FSmartObjectRegistration` and `FMassActorInstanceFragment` fragments when the a given tag or fragment is added to an entity.
See `FSmartObjectRegistrationFragment` and `FMassActorInstanceFragment`.

# Networking
## `UMassReplicationProcessor`
Base processor that handles replication and only runs on the server. 
You should derive from this per entity type (that require different replication processing). It and its derived classes query Mass entity fragments and set those values for replication when appropriate, using the MassClientBubbleHandler.

## `UMassNetworkIDFragmentInitializer`

## `UMassReplicationGridProcessor`
Processor to update entity in the replication grid used to fetch entities close to clients.

## `UMassReplicationGridRemoverProcessor`
Deinitializer processor to remove entity from the replication grid.

# Translator
## `UMassTranslator`
A class that's responsible for translation between UObjects and Mass. A translator knows how to initialize fragments related to the UClass that the given translator cares about. It can also be used at runtime to copy values from UObjects to fragments and back.

## `UMassCapsuleTransformToMassTranslator`

## `UMassTransformToActorCapsuleTranslator`

## `UMassCharacterMovementToActorTranslator`

## `UMassCharacterMovementToMassTranslator`

## `UMassCharacterOrientationToActorTranslator`

## `UMassCharacterOrientationToMassTranslator`
## `UMassSceneComponentLocationToActorTranslator`
## `UMassSceneComponentLocationToMassTranslator`
## `UMassTranslator_BehaviorTree`

# `Debug`
## `UAssignDebugVisProcessor`

## `UDebugVisLocationProcessor`

## `UMassProcessor_UpdateDebugVis`

# Miscs

## `UMassSignalProcessorBase`
Processor for executing signals on each targeted entities.
The derived classes only need to implement the method SignalEntities to actually received the raised signals for the entities they subscribed to.

## `UMassSpawnLocationProcessor`

## `UMassCompositeProcessor`
