
# Resources
- [RenderDoc plugin](https://dev.epicgames.com/documentation/en-us/unreal-engine/using-renderdoc-with-unreal-engine)

# Commands and more
- `stat gpu`
- `state scenerendering`
- `r.VisualizeOccludedPrimitives 1`
![[Pasted image 20250730054408.png]]
- `FreezeRendering`, good to use with `ToggleDebugCamera`, allows you to see how culling works, with extra stuff like seeing the POV of different steps of rendering (press B)
- `ToggleDebugCamera`
- `Foliage.Freeze`: Pauses the current rendering state of occluded and visible painted foliage clusters in Level based on the camera view. (And `Foliage.Unfreeze`)
- `ProfileGPU` panel, also named "GPU Visualizer"
- `r.Streaming.PoolSize XXX`
- `r.VisualizeOccludedPrimitives` and `r.AllowOcclusionQueries`

## Lumen
- `r.Lumen.DiffuseIndirect.Allow` Allows you to turn on or off lumen GI.

