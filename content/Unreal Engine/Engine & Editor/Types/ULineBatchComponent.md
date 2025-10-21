
# General usage in the engine
`UWorld` has 3 line batcher components:
- `LineBatcher`: "Line Batchers. All lines to be drawn in the world."
- `PersistentLineBatcher`: "Persistent Line Batchers. They don't get flushed every frame"
- `ForegroundLineBatcher`: "Foreground Line Batchers. This can't be Persistent."

There is also a single LineBatcher made in the `FPreviewScene`.

In `DrawDebugHelpers` `GetDebugLineBatcher` is used to get what instance to use. `ForegroundLineBatcher` is picked if the depth is strictly equal to 1.
`PersistentLineBatcher` is picked if the draw is marked as persistent or the draw time is higher than 0.
Otherwise for single frame draws `LineBatcher` is used.


They are all from the same type, and set up the same.

**LineBatcher**
- Has `bCalculateAccurateBounds` set to false.
- Is used by Niagara, HUD, Viewport, WorldPartition, SkeletalMeshComponent

**PersistentLineBatcher**
- Has `bCalculateAccurateBounds` set to false
- Is used by DrawPrimitiveDebugger

**ForegroundLineBatcher**
- Has `bCalculateAccurateBounds` set to false
- Is used by Viewport, CameraDebugRenderer

# Config
- `bCalculateAccurateBounds`: "Whether to calculate a tight accurate bounds (encompassing all points), or use a giant bounds that is fast to compute.". This is true by default and is used in `ULineBatchComponent::CalcBounds`.

# Process
On tick the component updates the lifetime of the batched elements and removed expired elements. If at least 1 element was removed `MarkRenderStateDirty` is called.

# Render
- In each `DrawXXX` function `UActorComponent::MarkRenderStateDirty` is called.
- `FLineBatcherSceneProxy` is the proxy scene type of the line batcher component.
- All batches points, lines and meshes has a `uint32 BatchID` that is set to 0 (`INVALID_ID`) by default. Use `ULineBatchComponent::ClearBatch` to clear it.
- `FLineBatcherSceneProxy::GetDynamicMeshElements` is what draws the points and lines on the PrimitiveDrawInterface.
