
You can use `r.AllowSubPrimitiveQueries` to allow multiple occlusion queries for one proxy.

`FPrimitiveSceneInfo::AddToScene` seems to be were most primitives bounds are queried and stored in the `FScene` `PrimitiveOcclusionBounds` array.

