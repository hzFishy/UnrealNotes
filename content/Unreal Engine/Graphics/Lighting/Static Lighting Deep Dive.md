
# Types and Vars
## Miscs types
- `FStaticLightingSystemInterface`
- `FStaticLightingManager` - Manages systems
- `FStaticLightingSystem` - Runs Lightmass processor and handles the start/process/end of applying build data.
- `FLightmassProcessor` / `FLightmassExporter`
- `FStaticLightingBuildContext` - Created along `FStaticLightingSystem`
- `UMapBuildDataRegistry` - Per level, the actual asset you can see in the Content Browser.
- `FMeshMapBuildData`
- `FLightMap2D` (and `FLightMap`)
- `FShadowMap2D` (and `FShadowMap`)
- `ULightMapTexture2D`
- `FLightmapResourceCluster`
-  `FStaticLightingMapping` / `FStaticLightingTextureMapping` / `FStaticLightingTextureMapping_InstancedStaticMesh` / `FBSPSurfaceStaticLighting` / `FStaticLightingGlobalVolumeMapping` / `FStaticMeshStaticLightingTextureMapping` / `FLandscapeStaticLightingGlobalVolumeMapping` / `FLandscapeStaticLightingTextureMapping`
- `FStaticLightingMesh`
- `FStaticLightingPrimitiveInfo`
- `ALightmassImportanceVolume` (A bounding volume outside of which Lightmass photon emissions are decreased) / `ALightmassCharacterIndirectDetailVolume` (Defines areas where Lightmass should place more indirect lighting samples than normal) / `AVolumetricLightmapDensityVolume` ( Provides local control over volumetric lightmap density.  Only convex shapes supported) / `ULightmassPortalComponent`.
- `FLightPrimitiveInteraction`

## Miscs Globals
- `FDebugLightingOutput GDebugStaticLightingInfo` (Initialized in `FStaticLightingSystem::BeginLightmassProcess`)`
- `GLightmapCounter` `GNumLightmapTotalTexels`, `GNumLightmapTotalTexelsNonPow2`, `GNumLightmapTextures`, `GNumLightmapMappedTexels`, `GNumLightmapUnmappedTexels`, `GLightmapTotalSize`, `GLightmapTotalStreamingSize`, `GNumShadowmapTotalTexels`, `GNumShadowmapTextures`, `GNumShadowmapMappedTexels`, `GNumShadowmapUnmappedTexels`, `GShadowmapTotalSize`, `GShadowmapTotalStreamingSize`, `GLightingBuildQuality`, `GCurrentSelectedLightmapSample` are all initialized in `FStaticLightingSystem::BeginLightmassProcess`.
- `GPurgeOldLightmaps` is controlled by `PurgeOldLightmaps` console var. 1 by default.

## Miscs functions
- `UWorld::GetLightMapsAndShadowMaps`

## Miscs delegates
- `FEditorDelegates::OnLightingBuildStarted` - Called when a lighting build has started
- `FEditorDelegates::OnLightingBuildKept` - Called when a lighting build has been kept
- `FEditorDelegates::OnLightingBuildFailed` - Called when a lighting build has failed (maybe called twice if cancelled)
- `FEditorDelegates::OnLightingBuildSucceeded` - Called when a lighting build has succeeded

- `FStaticLightingSystemInterface::OnPrimitiveComponentRegistered`
- `FStaticLightingSystemInterface::OnPrimitiveComponentUnregistered`
- `FStaticLightingSystemInterface::OnLightComponentRegistered`
- `FStaticLightingSystemInterface::OnLightComponentUnregistered`
- `FStaticLightingSystemInterface::OnStationaryLightChannelReassigned`
- `StaticLightingSystemInterface::OnLightmassImportanceVolumeModified`
- `FStaticLightingSystemInterface::OnMaterialInvalidated`
- `FStaticLightingSystemInterface::OnSkyAtmosphereModified`

# Tools
You can see the baked light static mesh info here.<br>
![[Pasted image 20261113032955.png]]

# Static Lighting
## Core
The `FStaticLightingManager` always exists as its Get method creates an instance if none exists.

`FStaticLightingManager::CreateStaticLightingSystem` is used to create new `FStaticLightingSystem` instances. The only usage I found is in `UEditorEngine::BuildLighting`.  `FStaticLightingBuildContext` is also created there.

## Building
### Start
To build light `UEditorEngine::BuildLighting` is called.

One of the paths to run it is `ULevelEditorSubsystem::BuildLightMaps`.
After creating a new `FStaticLightingSystem`, `FStaticLightingSystem::BeginLightmassProcess` is called.

#### `FStaticLightingSystem::BeginLightmassProcess`

Inside this function a lot of process is done. 

---

First all `UPrimitiveComponent` with a valid `VisibilityId` are added to a `PackagesToDirty` array. Which later is iterated to mark them as dirty.

---

Then a `TMap<FGuid, ULevel*> LevelGuids` map is filled. **IT IS ONLY USED TO CHECK THAT EACH LEVEL HAS A UNIQUE `LevelBuildDataId`**
For each `ULevels` stored in the built main world `ShouldOperateOnLevel` (Check if the level is valid, if  `LightingContext.ShouldIncludeLevel` passes and if the level is `bIsVisible`) and `ShouldBuildLightingForLevel` (Checks if the level is valid, checks for `bOnlyBuildCurrentLevel` and `bOnlyBuildSelectedLevels`) is ran to know if it should include it later for the build process.
The `FGuid` taken is `ULevel::LevelBuildDataId`. This id is initialized in `ULevel::PostInitProperties`.

---

Next a `SkippedLevels` string is filled depending on a set of rules and `GatherBuildDataResourcesToKeep`. This seems to only be relevant with lighting scenarios. It interacts with `BuildDataResourcesToKeep`

---

Next some special options for World Partition (`bApplyDeferedActorMappingPass` and `bVolumetricLightmapFinalizerPass`) are used to do some extra work if set. It interacts with `BuildDataResourcesToKeep`

---

Next streaming levels are iterated. For each level it will be ignored if its not loaded or not visible in editor. Other than that the streaming level world asset package name is appended to the `SkippedLevels` string.

---

At this point a warning message will inform the user than some levels are skipped if `SkippedLevels` is not empty.

---

Next we have some settings being read.

The system gets `r.AllowStaticLighting`.

The system gets `bForceNoPrecomputedLighting` in the world settings.

The system reads `Engine.ini`, in `TextureStreaming` section the `MaxLightmapRadius` var and write it to `GMaxLightmapRadius`.

The system reads `Engine.ini`, in `TextureStreaming` section the `AllowStreamingLightmaps` var and write it to `GAllowStreamingLightmaps`.

The system reads `Engine.ini`, in `DevOptions.StaticLighting` section the `bUseBilinearFilterLightmaps` var and write it to `GUseBilinearLightmaps`.

The system reads `Engine.ini`, in `DevOptions.StaticLighting` section the `bAllowCropping` var and write it to `GAllowLightmapCropping`.

The system reads `Engine.ini`, in `DevOptions.StaticLighting` section the `bRebuildDirtyGeometryForLighting` var and write it to `bRebuildDirtyGeometryForLighting`.

The system reads `Engine.ini`, in `DevOptions.StaticLighting` section the `bCompressLightmaps` var and write it to `GCompressLightmaps`.

---

Next, if `bOnlyBuildVisibility` in the option isn't true we do 2 things.

1: 
We delete all `AGeneratedMeshAreaLight` actors, this seems to be old engine feature than isn't used now.

2: 
We iterate all `ULightComponentBase` objects that aren't garbage or CDOs, we filter them to be sure to only have the lights that our world holds and that their owner actor is valid.
We check again for `ShouldOperateOnLevel` on the owner actor level.

The light component is added to the `Lights` array if all previous conditions passes and if `bAffectsWorld` is true, and either `ULightComponentBase::HasStaticShadowing` or `ULightComponentBase::HasStaticLighting` returns true (Those just check for `Mobility`).

---

Next, `FStaticLightingSystem::GatherStaticLightingInfo` is called. This function alone is huge so it will be detailed in its separate section.

---

Next if `GLightmassDebugOptions.bSortMappings` is true we will sort the `UnSortedMappings` into `Mappings`. (Those mapping are filled by `GatherStaticLightingInfo`).

---

If `bForceNoPrecomputedLighting` is true `FStaticLightingSystem::InvalidateStaticLighting` is called.
This basically invalidates anything that could be set by a previous computation (`Level->MapBuildData->InvalidateStaticLighting`, `Level->PrecomputedVisibilityHandler`,  `Level->PrecomputedVolumeDistanceField` and `MarkPlatformDataTransient` for all `ULightMapTexture2D` that has the `Level->MapBuildData` as outer package, all of those are called for each level in the main world).

If `bForceNoPrecomputedLighting` is false `FStaticLightingSystem::CreateLightmassProcessor` is called (this just create a `FLightmassProcessor* LightmassProcessor`) as well as `FStaticLightingSystem::GatherScene` and `FStaticLightingSystem::InitiateLightmassProcessor`. Check their own sections for more info.

#### `FStaticLightingSystem::GatherStaticLightingInfo`

In here we iterate all levels hold by the main world and check for `ShouldOperateOnLevel`.

It checks if it should call `UEditorEngine::RebuildMap`  with rebuild type `EMapRebuildType::MRT_AllDirtyForLighting` depending on `Level->bGeometryDirtyForLighting` and `bRebuildDirtyGeometryForLighting`

---

Next BSP and Models will be iterated, I won't go into details here since BSP are kind of deprecated in UE5.

---

Next HLOD actors (`ALODActor`) are iterated, we fill a `PrimitiveActorMap` and `PrimitiveSubStaticMeshMap` multi map.

---

Next we iterate all actors. Multiple things happens here:

For each actor we get its primitive components. And we check if `PrimitiveActorMap` contains the actor (meaning the actor was the `SubActor` of another `ALODActor`). Also if the actor is a `ALODActor` we add it to the `LODActors` array. If the actor is a `AWorldPartitionHLOD` we add it to the `WorldPartitionHLODActors` array.

For each primitive component we iterate the previously filled `Lights` array and for each `ULightComponent` that passes `ULightComponent::AffectsPrimitive` we store it in `PrimitiveRelevantLights`.

Then we call `UPrimitiveComponent::GetStaticLightingInfo` and we pass the `PrimitiveRelevantLights` array. This outputs a `FStaticLightingPrimitiveInfo PrimitiveInfo`.

If `bPrecomputeVisibility` is true in the world settings some extra stuff is done with the primitive `VisibilityId`.

Each `PrimitiveInfo.Meshes` is added to the `ActorMeshMap` multi map.

`FStaticLightingSystem::AddPrimitiveStaticLightingInfo` is called.

---

Next for each `LODActors` we call `FStaticLightingSystem::UpdateStaticLightingHLODTreeIndices`.

And for each `WorldPartitionHLODActors` we call `FStaticLightingSystem::UpdateStaticLightingWorldPartitionHLODTreeIndices`.

##### `UStaticMeshComponent::GetStaticLightingInfo`

*Incomplete*

We iterate each `GetStaticMesh()->GetRenderData()->LODResources` (`FStaticMeshLODResources`).

We then call `UStaticMeshComponent::UpdateStaticLightingData` which calls `FStaticMeshComponentLODInfo::CreateMapBuildDataId`. And we call `UStaticMeshComponent::UpdateMapBuildDataId` afterwards.

We then fill the `Meshes` and `Mapping` arrays.

##### `FStaticLightingSystem::AddPrimitiveStaticLightingInfo`

For each `PrimitiveInfo.Meshes` we:
- Add the `PrimitiveInfo.VisibilityId` to `Mesh->VisibilityIds`.
- Add the Mesh to the `Meshes` array.
- Add the `Mesh->BoundingBox` to `LightingMeshBounds`.
- If `Mesh->bCastShadow` we call `FStaticLightingSystem::UpdateAutomaticImportanceVolumeBounds` which just updates the `AutomaticImportanceVolumeBounds` `FBox`.

For each `PrimitiveInfo.Mappings` we:
- Add it to the `Mappings` array.

#### `FStaticLightingSystem::GatherScene`

This function is all about getting the Lightmass Exporter and filling it from all the scene data we got until now.
It also sets its level settings and quality level.

---

Then if we aren't a world partitioned world, we add all `ALightmassImportanceVolume` in the world to the Lightmass Exporter. Otherwise we manually enter a importance volume bounding box using volumetric light map grid desc (`FVolumetricLightMapGridDesc`).

---

Next we iterate all `ALightmassCharacterIndirectDetailVolume` actor in the world and add it to the Lightmass Exporter.

Next we iterate all `AVolumetricLightmapDensityVolume` actor in the world and add it to the Lightmass Exporter.

Next we iterate all `ULightmassPortalComponent` component in the world and add it to the Lightmass Exporter.

Next we iterate all `USkyAtmosphereComponent` component in the world and we ONLY add the first one found to the Lightmass Exporter.

---

The system reads `Engine.ini`, in `DevOptions.StaticLightingSceneConstants` section the `MinimumImportanceVolumeExtentWithoutWarning` var and write it to `MinimumImportanceVolumeExtentWithoutWarning`.

We will use that to warn the user if the level is to big and we have no importance volume actor.

---

Next for each `Meshes` we call `FStaticLightingMesh::ExportMeshInstance`. Internally (for example for a `FStaticMeshStaticLightingMesh`) this add the `RelevantLights` to the exporter, as well as the static meshes and materials.

Next for each `Mappings` we call `FStaticLightingMapping::ExportMapping`.

### Running
On each `UEditorEngine::UpdateBuildLighting` call, `FStaticLightingManager::UpdateBuildLighting` runs `FStaticLightingSystem::UpdateLightingBuild` on the active system.

`UEditorEngine::UpdateBuildLighting` is called from multiple places, including `UUnrealEdEngine::Tick` and `ULevelEditorSubsystem::BuildLightMaps` in a `while` loop (the one after calling `UEditorEngine::BuildLighting`, using `UEditorEngine::IsLightingBuildCurrentlyRunning`).

### End
At the end of the lightmass process, `FStaticLightingManager::ProcessLightingData` is called.
This will run `FStaticLightingSystem::FinishLightmassProcess` on the system.
This function does a number of important things:

**1.`FStaticLightingSystem::InvalidateStaticLighting`** - Invalidates the lighting of the current levels so new lighting can be applied.<br>
It iterates all levels for the Lighting context world, if it should update the static Lighting data it will call `ULevel::ReleaseRenderingResources` and clear `MapBuildData` using `UMapBuildDataRegistry::InvalidateStaticLighting`.
Some others miscellaneous stuff is done inside the function.

**0.5`FLightmassProcessor::CompleteRun`**<br>
A lot of things happens inside, here I will only mention that it can eventually call `FLightmassProcessor::ProcessMapping` which calls `FStaticLightingSystem::ApplyMapping` that internally calls the `Apply` virtual of `FStaticLightingMapping`. In most cases it will be an `FStaticMeshStaticLightingTextureMapping` if its a SM.
All of this dive to say that `UMapBuildDataRegistry::AllocateMeshBuildData` will be executed to add a `MeshBuildData` entry with an defaulted ("empty") `FMeshMapBuildData`. 

**2.`FStaticLightingSystem::CompleteDeterministicMappings`**<br>
Uses the `Mappings` array of item type `FStaticLightingMapping`.
The `Mappings` variable is used if we have sorting enabled (`GLightmassDebugOptions.bSortMappings`). Otherwise `UnSortedMappings` is used, which its item type is `FStaticLightingMappingSortHelper`.
In my testing this was skipped so I can't tell much on what it does, check source code for more info.

**3.`FStaticLightingSystem::EncodeTextures`** - After importing, textures need to be encoded to be used.<br>
This will run `FLightMap2D::EncodeTextures` and `FShadowMap2D::EncodeTextures`.
This is also where the `ULightMapTexture2D` are created (see `FLightMapPendingTexture::CreateUObjects`).


**4.`FStaticLightingSystem::ApplyNewLightingData`** - Pushes newly collected lightmaps on to the level.<br>
For each considered level, this will:
1. Loop 1:
	- Call `ULevel::OnApplyNewLightingData` (only updates `LightBuildLevelOffset`)
	- If persistent level call `UpdateScene` on `PrecomputedVisibilityHandler` and `PrecomputedVolumeDistanceField`.
	- For each actor:
		- Get `UMapBuildDataRegistry` using `FStaticLightingBuildContext::GetOrCreateRegistryForActor`.
		- For each light component:
			- If `UMapBuildDataRegistry::GetLightBuildData` fails it calls `UMapBuildDataRegistry::FindOrAllocateLightBuildData`
		- For each sky atmosphere component:
			- If `UMapBuildDataRegistry::GetSkyAtmosphereBuildData` fails it calls `UMapBuildDataRegistry::FindOrAllocateSkyAtmosphereBuildData`.
2. Loop 2:
	- Get `UMapBuildDataRegistry` using `FStaticLightingBuildContext::GetRegistryForLevel`
	- Runs `UMapBuildDataRegistry::SetupLightmapResourceClusters` on the registry.
	- Runs `ULevel::InitializeRenderingResources`

## More on `UMapBuildDataRegistry`
So now our `UMapBuildDataRegistry` holds our built data. Such as an array `MeshBuildData` holding `FMeshMapBuildData`, and an array `LightBuildData` of holding `FLightComponentMapBuildData`.
An `FMeshMapBuildData` holds data such as a `FLightMapRef` and a `FShadowMapRef`.

The `FLightMap2D` holds an array of `Textures` of type `ULightMapTexture2D`.
The first entry is a High Quality (HQ) lightmap and the second is a Low Quality (LQ) lightmap (When loading up a level with Static Lighting you can see a rough quality change after a few seconds when the editor is loading the HQ after using the LQ).

## Usage
So ... how is this used ?

On `UMapBuildDataRegistry::PostLoad` the clusters are created and sent to the render thread (see `UMapBuildDataRegistry::SetupLightmapResourceClusters` and check `FLightmapResourceCluster` section below).

In my simulation (empty world with point light and floor SM), here is how light maps are queried:

**By the Spot Light:**
1. `ULightComponent::CreateRenderState_Concurrent` is called.
2. This internally calls `FScene::AddLight` on the world scene, which calls `UPointLightComponent::CreateSceneProxy`
3. Inside the `FLightSceneProxy` constructor `ULightComponent::GetLightComponentMapBuildData` is called. This runs the `UMapBuildDataRegistry::Get` overload using a Actor Component, which internally uses the one using a Level and World as input if you aren't using World Partition. If the light component was previously built this will return a valid `FLightComponentMapBuildData` ptr (using `UMapBuildDataRegistry::GetLightBuildData`).

**By the floor Static Mesh:**
1. `UStaticMeshComponent::CreateStaticMeshSceneProxy` is called.
2. Inside the `FStaticMeshSceneProxy` constructor `UStaticMeshComponent::IsPrecomputedLightingValid` will be called to set a bool.
3. This will call `UStaticMeshComponent::GetMeshMapBuildData`, which will use the `FGuid MapBuildDataId` stored in the LOD to get the Mesh Build Data.


# Miscs

**`FStaticLightingDescriptors`**<br>
`Descriptors` is created in `FStaticLightingBuildContext::FStaticLightingBuildContext` IF the world is using World Partition.

**`FLightmapResourceCluster`**<br>
A bundle of lightmap resources which are referenced by multiple components.

Comment on `FLightmapResourceCluster::TryInitializeUniformBuffer`.
```c++
// Two stage initialization of FLightmapResourceCluster  
// 1. when UMapBuildDataRegistry is post-loaded and render resource is initialized
// 2. when the level is made visible (ULevel::InitializeRenderingResources()), which calls UMapBuildDataRegistry::InitializeClusterRenderingResources() and fills FeatureLevel  
// When both parts are provided, TryInitialize() creates the final UB with actual content  
// Otherwise UniformBuffer is created with empty parameters
```

This seems to be what is used by the render thread to know what to use.


**World Settings displayed "Lightmaps"** <br>
In the world settings panel, the "Lightmaps" row in the Advanced area isn't a variable but a custom row injected from `FLightmapCustomNodeBuilder`.
The text is formatted from `FLightmapCustomNodeBuilder::GetLightmapCountText`, which uses the `LightmapItems` array.
The array is recreated on each `FLightmapCustomNodeBuilder::RefreshLightmapItems` call.
The light maps and shadow maps are queried by using `UWorld::GetLightMapsAndShadowMaps`.


**More on `UWorld::GetLightMapsAndShadowMaps`**<br>
`UWorld::GetLightMapsAndShadowMaps` works by getting all `ULightMapTexture2D`, `UShadowMapTexture2D` and `ULightMapVirtualTexture2D` objects.


**More on `ULevel`**<br>
In `ULevel` there is `MapBuildData` of type `UMapBuildDataRegistry`.
