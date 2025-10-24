
# Mass Visualization Traits

> [!Info] Available Types
> - `UMassStationaryVisualizationTrait`
> - `UMassMovableVisualizationTrait`

> [!Warning] MassStationaryDistanceVisualizationTrait
> **MassStationaryDistanceVisualizationTrait** has been soft deprecated (its parent **MassDistanceVisualizationTrait** is marked as soft deprecated in source code)

## Differences
The main difference between the movable and stationary type is that `UMassStationaryVisualizationTrait` sets the `Mobility` of the `FMassStaticMeshInstanceVisualizationMeshDesc` to `Stationnary`
 (default is `Movable`).

You can see this `Movable` property be used in `UMassVisualizationComponent::ConstructStaticMeshComponents` to set the mobility of the ISM.

## About
This allows you to swap at runtime between a "High Res" Actor, "Low Res" Actor and One or more static meshes based on your LOD rules.

The default LOD rules are distance based.

**Static Mesh Instance Desc** holds the data for the static meshes you want to spawn (same parent transform for all, can customize an offset (see **Local Transform**) for each mesh).

Inside **Params** you can set what visualization you want to use inside **LODRepresentation**.
For example when the player is close the High Res Actor is used, when it goes far away it will use the Static Meshes and when very far nothing will be used.

![[Pasted image 20251024095734.png]]

Inside **Params**, if **Keep Low Res Actors** is true the spawned Low Res Actors will not be destroyed when we go further down in the LOD representations (for example from Medium to Low). But if will be "disabled".

By "disabled" we talk about `EMassActorEnabledType`. Which in this case is used in `UMassRepresentationActorManagement::SetActorEnabled`.
By default a "disabled" actor has its tick disabled (using `AActor::SetActorTickEnabled`) and its collisions disabled (using `AActor::SetActorEnableCollision`).


Inside **LODParams** you can control what LOD to use on what distance inside **Base LODDistance**.

In this example (using the screen **LOD Representation** above) we would have the High Res Actor between 0 and 5m, Low Res Actor between 5m and 10m, the Static Meshes between 10m and 100m and nothing above 100m.

![[Pasted image 20251024101206.png]]

**Visible LODDistance** is a bit more complex and can be combined with the "Frustum" params to control the culling.

**LODMax Count** lets you control (if needed) how many entities you allow to exist for each LOD category.
For example: "I only want 5 High Res Actors" will turn the 5 "best" entities into High Res Actors, and the other actors will stay at their level (for example Low Res Actor).
