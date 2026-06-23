
# Minimal setup

## Scene Proxy
Subclass `FDebugRenderSceneProxy`.

Implement a constructor, default is `MyProxy(const UPrimitiveComponent* InComponent)`.

Override and implement `GetViewRelevance`.
```c++
// Default implementation, bDynamicRelevance is required so GetDynamicMeshElements is called on the proxy
FPrimitiveViewRelevance Result;
Result.bDrawRelevance = IsShown(View);
Result.bDynamicRelevance = true;
return Result;
```

If the proxy holds variables you will have to override other functions such as `GetMemoryFootprint` and `GetAllocatedSize`.

## Primitive Component

Subclass `UDebugDrawComponent`.

Override and implement `CreateDebugSceneProxy`.

Override and implement `CalcBounds`.

## Render text
See `FDebugDrawDelegateHelper` (implementation code example at `UDebugDrawComponent` declaration).

By default the text is drawn in `Game` mode, you can change that by editing the `ViewFlagName` variable (see `FDebugDrawDelegateHelper::RegisterDebugDrawDelegateInternal`).
The var is set in `FDebugDrawDelegateHelper::InitDelegateHelper` from the proxy `ViewFlagName` var so you should set the `ViewFlagName` in the proxy and not directly in the delegate helper.

To setup a custom show flag see [[Custom show flag]].

