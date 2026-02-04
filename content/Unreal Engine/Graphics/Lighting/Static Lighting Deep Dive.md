
# Resources
## Important types
- `FStaticLightingManager` - Manages systems
- `FStaticLightingSystem` - Runs Lightmass processor and handles the start/process/end of applying build data.
- `FLightmassProcessor`
- `FStaticLightingBuildContext` - Created along `FStaticLightingSystem`
- `UMapBuildDataRegistry` - Per level, the actual asset you can see in the Content Browser.
- `FMeshMapBuildData`
- `FLightMap2D` (and `FLightMap`)
- `FShadowMap2D` (and `FShadowMap`)
- `ULightMapTexture2D`
- `FLightmapResourceCluster`
- `FStaticLightingTextureMapping`

## Important functions
- `UWorld::GetLightMapsAndShadowMaps`

## Important delegates
- `FEditorDelegates::OnLightingBuildStarted` - Called when a lighting build has started
- `FEditorDelegates::OnLightingBuildKept` - Called when a lighting build has been kept
- `FEditorDelegates::OnLightingBuildFailed` - Called when a lighting build has failed (maybe called twice if cancelled)
- `FEditorDelegates::OnLightingBuildSucceeded` - Called when a lighting build has succeeded

# Static Lighting
## Core
The `FStaticLightingManager` always exists as its Get method creates an instance if none exists.

`FStaticLightingManager::CreateStaticLightingSystem` is used to create new `FStaticLightingSystem` instances. The only usage I found is in `UEditorEngine::BuildLighting`.  `FStaticLightingBuildContext` is also created there.

## Building
### Start
To build light `UEditorEngine::BuildLighting` is called.
One of the paths to run it is `ULevelEditorSubsystem::BuildLightMaps`.
After creating a new `FStaticLightingSystem`, `FStaticLightingSystem::BeginLightmassProcess` is called.

Inside this function a lot of process is done, `Lights` array is filled by getting all `ULightComponentBase` that is contained in the built world.
The light component also needs to be affecting the world, as well as allowing static shadowing or static lightening.

`FStaticLightingSystem::GatherStaticLightingInfo` is also called inside.

`FStaticLightingSystem::GatherScene` is later called as well as `FStaticLightingSystem::InitiateLightmassProcessor`.

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
