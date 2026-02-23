


# Objects

See `TObjectIterator`

Examples:
```c++
// Default
for (TObjectIterator<UPSPrefabReferenceComponentBase> It; It; ++It)

// with flags (and more if needed)
for (auto It = TObjectIterator<UPSPrefabReferenceComponentBase>(EObjectFlags::RF_ClassDefaultObject | EObjectFlags::RF_ArchetypeObject); It; ++It)
```

# Actors

Recommended to read [this article section](https://dev.epicgames.com/community/learning/tutorials/l3E0/myth-busting-best-practices-in-unreal-engine#let%E2%80%99stalkaboutgetallactorsofclass).

## `GetAllActorsOfClass`
This uses the `TActorIterator` if a subclass is diven.

## `GetAllActorsWithInterface` & `GetAllActorsWithTag`
This is slow if you have a big world, because it will check if ALL actors in the world has the interface/tag.
You can see it in source code at `UGameplayStatics::GetAllActorsWithInterface`, `UGameplayStatics::GetAllActorsWithTag`.

## `TActorIterator`
Its usually fast, but it scales with the number of actors, because it runs some checks on each found actors.

```c++
for (auto It = TActorIterator<AActor>(World); It; ++It)
// or ...
for (TActorIterator<AActor> It(World); It; ++It)
```

**How it works**<br>
This is the interesting part of the declaration of the actor iterator: `class TActorIterator : public TActorIteratorBase<TActorIterator<ActorType>>`.

The iteration process happens in `TActorIteratorBase` when using the `++` operator (see `void operator++()`). This is a while loop that uses the `LocalObjectArray = State->ObjectArray` array.

`TActorIteratorBase` has a `State` property of type `FActorIteratorState`. it is constructed in the `TActorIteratorBase` constructor using an `.Emplace`.

In the `FActorIteratorState` constructor it will fill properties such as the `ObjectArray` using `GetObjectsOfClass` (In Editor there is some extra code because of multiple worlds being possible).
After that, it is registering a `OnActorSpawned` function using `AddOnActorSpawnedHandler` on the iterator world. Probably in case some new actors are spawned while we are iterating.
