
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
