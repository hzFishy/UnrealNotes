
# Resources
- [UE Docs: GPU Lightmass Global Illumination](https://dev.epicgames.com/documentation/unreal-engine/gpu-lightmass-global-illumination-in-unreal-engine)
- [Updated GPU Lightmass patch by ex-epic dev](https://forums.unrealengine.com/t/epics-gpulightmass/139357/667)


# Deep Dive

## Sources

### Miscs Types
- `IStaticLightingSystemImpl`
- `UGPULightmassSubsystem`
- `FGPULightmass` / `IStaticLightingSystem`
- `GPULightmass::FScene`
- `GPULightmass::FStaticMeshInstance` / `GPULightmass::FGeometry`
- `GPULightmass::FLightmap`

## Launch
When clicking on the "Build Lighting" button `FGPULightmassEditorModule::OnStartClicked` is called, this calls `UGPULightmassSubsystem::Launch`.

It will create a `FGPULightmass` instance with `FGPULightmassModule::CreateGPULightmassForWorld`.

When a new GPULM instance is made it will register to a bunch of Game Thread events, see `FGPULightmass::InstallGameThreadEventHooks` (binds to `FStaticLightingSystemInterface` delegates).

If the GPULM instance is valid, we call `FStaticLightingSystemInterface::OnPrimitiveComponentRegistered` on all `UPrimitiveComponent` and `FStaticLightingSystemInterface::OnLightComponentRegistered` on all `ULightComponentBase`.
The goal of this part is to trigger GPULM callbacks such as `FGPULightmass::OnPrimitiveComponentRegistered`, which calls `FScene::AddGeometryInstanceFromComponent`.

For example for a static mesh this will call `FStaticMeshInstance::AllocateLightmaps``.

# Tick

GPULightmass is ticked on the GT using `GEngine->OnPostEditorTick`, binding it to `FStaticLightingSystemInterface::GameTick`.
This calls `FStaticLightingSystemInterface::EditorTick` which will try to call `IStaticLightingSystemImpl::EditorTick`.

`FGPULightmassModule::EditorTick` calls for each `StaticLightingSystems` `FGPULightmass::EditorTick` and if `LightBuildPercentage` reached 100 and the mode isn't `EGPULightmassMode::BakeWhatYouSee` it will call `UGPULightmassSubsystem::Stop`.

# End

See **Tick** section.

---

`GPULightmass::FScene::ApplyFinishedLightmapsToWorld` is called. Which calls `UMapBuildDataRegistry::InvalidateStaticLighting` for all invisible normal levels on its `MapBuildData`.

It will then allocate build data, for example for a static mesh component it will call `UMapBuildDataRegistry::AllocateMeshBuildData` and `FLightMap2D::AllocateLightMap`.

And for a point light it will call `FLocalLightBuildInfo::AllocateMapBuildData` which calls `UMapBuildDataRegistry::FindOrAllocateLightBuildData`.

---

`UGPULightmassSubsystem::Stop` will first call `FStaticLightingSystemInterface::OnPrimitiveComponentUnregistered` on all `ULandscapeComponent` and `UPrimitiveComponent`. And `FStaticLightingSystemInterface::OnLightComponentUnregistered` on all `ULightComponentBase`.

