See also [[Navigation Data]].


> [!Info] Note on `IsNavigationRelevant`
> The virtual function `UActorComponent::IsNavigationRelevant` which is mostly known for its implementation in `UPrimitiveComponent` will by default return true if the primitive has the channels `Pawn` and `Vehicle` as blocked.

# Registration process
When a primitive component is registered and `bCanEverAffectNavigation` is true as well as `IsNavigationRelevant`, `FNavigationSystem::OnComponentRegistered` is called.
This will call `UNavigationSystemV1::RegisterComponentToNavOctree`, which checks that the component implements `INavRelevantInterface` and checks `AActor::IsComponentRelevantForNavigation`.

If the nav generation type is not static, the following happens: 
`UNavigationSystemV1::RegisterNavRelevantObjectStatic` is called which calls `UNavigationObjectRepository::RegisterNavRelevantObject`.
This internal creates a new `FNavigationElement` instance (see `UNavigationObjectRepository::RegisterNavRelevantObjectInternal` and `FNavigationElement::InitializeFromInterface`).

`FNavigationSystem::OnComponentUnregistered` is called when the component unregistered (as well as some similar path that the register one).

# Refreshing the nav data
In editor when you update a component collisions `UActorComponent::DestroyPhysicsState` will be eventually called. This will trigger a callback to `UNavigationSystemV1::UpdateComponentInNavOctree` which calls `UNavigationObjectRepository::UpdateNavigationElementForUObject`, this will find an existing `FNavigationElement` in `UNavigationObjectRepository::RegisterNavRelevantObjectInterna` and override it with a new "instance".


# Generation process
See also [Static NavMesh Generation by Hussein Khalil](https://www.unrealdoc.com/i/132416769/logic-and-data-flow).


