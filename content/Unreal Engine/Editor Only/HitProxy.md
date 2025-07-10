
This is what is used by the engine to detect when you click on an object in a viewport.

# Types
- `HHitProxy`: Base Class
- `HActor`: For Actor & Component

# Creation process

For this example, we will see how a `HActor` proxy hit is created when you open a Blueprint (as each component has a Hit Proxy).

In this example, we look how the hit proxy is made for a Static Mesh Component.

After the SCS ran, `UActorComponent::ExecuteRegisterEvents` is called.
This runs `UActorComponent::CreateRenderState_Concurrent` (which here will also run `UStaticMeshComponent::CreateRenderState_Concurrent` and `UPrimitiveComponent::CreateRenderState_Concurrent`). This function is "`Used to create any rendering thread information for this component`".

`UPrimitiveComponent::CreateRenderState_Concurrent` is the important virtual override.
Since a primitive component has a "physical presence", it registers itself to the world `FScene` through `FScene::BatchAddPrimitivesInternal`.

> [!Info] So how can I select components that aren't primitives ?
> The Billboard Component is used, since its a primitive component to.
> See [[Billboard & Sprite Component]]

It will create a `FPrimitiveSceneInfo` for the primitive component, which creates internally an `FPrimitiveSceneInfoAdapter`. The latter will run `FPrimitiveSceneInfoAdapter::CreateHitProxies` if we are in the Editor.
This calls `FPrimitiveSceneProxy::CreateHitProxies` (the scene proxy is the one taken from `UPrimitiveComponent -> FPrimitiveSceneProxy* SceneProxy` var)

Eventually `FActorPrimitiveComponentInterface::CreatePrimitiveHitProxies` gets called, this is where the `HActor` are actually created.

The hit proxy is stored in `DefaultHitProxy`, inside the `FPrimitiveSceneInfoAdapter`.
In the `FPrimitiveSceneInfo` constructor, the `DefaultHitProxy` and `HitProxies` vars of the adapter are moved/copied to the `DefaultDynamicHitProxy` and `HitProxies` vars of the primitive scene info.

> [!Info] Access Hit Proxy from component
> ```c++
> auto* MainHitProxy = PrimitiveComponent->GetSceneProxy()->GetPrimitiveSceneInfo()->DefaultDynamicHitProxy; // There is also HitProxies var as mentioned above
> auto* ActorMainHitProxy = (HActor*)MainHitProxy;
> ```

