See also [[Navigation Data]].


> [!Info] Note on `IsNavigationRelevant`
> The virtual function `UActorComponent::IsNavigationRelevant` which is mostly known for its implementation in `UPrimitiveComponent` will by default return true if the primitive has the channels `Pawn` and `Vehicle` as blocked.

# Registration process
When a primitive component is registered and `bCanEverAffectNavigation` is true as well as `IsNavigationRelevant`, `FNavigationSystem::OnComponentRegistered` is called.
This will call `UNavigationSystemV1::RegisterComponentToNavOctree`, which checks that the component implements `INavRelevantInterface` and checks `AActor::IsComponentRelevantForNavigation`.

If the nav generation type is not static, the following happens: 
`UNavigationSystemV1::RegisterNavRelevantObjectStatic` is called which calls `UNavigationObjectRepository::RegisterNavRelevantObject`.
This internal creates a new `FNavigationElement` instance (see `UNavigationObjectRepository::RegisterNavRelevantObjectInternal` and `FNavigationElement::InitializeFromInterface`).

`FNavigationSystem::OnComponentUnregistered` is called when the component unregistered (as well as some similar path that the register one).

# Refreshing the nav data
In editor when you update a component collisions `UActorComponent::DestroyPhysicsState` will be eventually called. This will trigger a callback to `UNavigationSystemV1::UpdateComponentInNavOctree` which calls `UNavigationObjectRepository::UpdateNavigationElementForUObject`, this will find an existing `FNavigationElement` in `UNavigationObjectRepository::RegisterNavRelevantObjectInterna` and override it with a new "instance".

# Geometry Export
The recast nav system gathers the geometry (called "Exporting" in the engine) to know if its relevant to generate walkable surfaces.

For example in `FNavigationOctree::AddNode`, if `bGatherGeometry` is true it executes the `GeometryExportDelegate` delegate if its synchronous (see `FNavigationOctree::IsLazyGathering`). The delegate is set in `UNavigationSystemV1::ConditionalPopulateNavOctree` and point to the static function `FRecastGeometryExport::ExportElementGeometry`.

Inside `RecastGeometryExport::ExportObject` is called, as well as `RecastGeometryExport::ConvertCoordDataToRecast` and `RecastGeometryExport::StoreCollisionCache`.

Inside `ExportObject`, `GeometryExportType` is checked, if not set to `No` then the `CustomGeometryExportDelegate` delegate of the processed `FNavigationElement` is called. This will call `INavRelevantInterface::DoCustomNavigableGeometryExport`.

For a `UStaticMeshComponent` this runs `FStaticMeshComponentHelper::DoCustomNavigableGeometryExport`. If the scale is zero it is skipped.

Then `UNavCollision::ExportGeometry` will be called.
This will call `ExportCustomMesh` on the passed `FNavigableGeometryExport` (in this case a `FRecastGeometryExport`) we are filling. The function internally calls `RecastGeometryExport::ExportCustomMesh`, which uses the input vertex and index data with the internal `VertexBufffer` and `IndexBuffer` it holds (see `FRecastGeometryExport` members).

At the end if no custom export data was found and if the nav element has a `BodySetup` `RecastGeometryExport::ExportRigidBodySetup` is called.

## Snippets

## Debug Exported Nav Geo
Here is a code snippet you can use to debug the exported nav geometry:
```c++
// Note: FU::Draw functions comes from my FishyUtils plugin.

bool USANTestStaticMeshComponent::DoCustomNavigableGeometryExport(FNavigableGeometryExport& GeomExport) const
{
	const bool bResult = Super::DoCustomNavigableGeometryExport(GeomExport);
	
	auto& RecastGeoExport = static_cast<FRecastGeometryExport&>(GeomExport);
	
	if (RecastGeoExport.Data)
	{
		// debug bounds
		FU::Draw::DrawDebugBox(
			GetWorld(),
			RecastGeoExport.Data->Bounds.GetCenter(),
			RecastGeoExport.Data->Bounds.GetExtent(),
			FQuat::Identity,
			FColor::Red,
			10
		);
	}
	
	// reconstruct exported mesh from vertex bufffer
	const int32 TriangleCount = RecastGeoExport.IndexBuffer.Num() / 3;
	for (int32 TriangleIndex = 0; TriangleIndex < TriangleCount; ++TriangleIndex)
	{
		const int32 IndexOffset = TriangleIndex * 3;
		
		const int32 Index1 = RecastGeoExport.IndexBuffer[IndexOffset];
		const int32 Index2 = RecastGeoExport.IndexBuffer[IndexOffset + 1];
		const int32 Index3 = RecastGeoExport.IndexBuffer[IndexOffset + 2];
		
		const FVector Pos1 = FVector(
			RecastGeoExport.VertexBuffer[Index1 * 3],
			RecastGeoExport.VertexBuffer[Index1 * 3 + 1],
			RecastGeoExport.VertexBuffer[Index1 * 3 + 2]
		);
		const FVector Pos2 = FVector(
			RecastGeoExport.VertexBuffer[Index2 * 3],
			RecastGeoExport.VertexBuffer[Index2 * 3 + 1],
			RecastGeoExport.VertexBuffer[Index2 * 3 + 2]
		);
		const FVector Pos3 = FVector(
			RecastGeoExport.VertexBuffer[Index3 * 3],
			RecastGeoExport.VertexBuffer[Index3 * 3 + 1],
			RecastGeoExport.VertexBuffer[Index3 * 3 + 2]
		);
		
		FU::Draw::DrawDebugLine(
			GetWorld(),
			Pos1,
			Pos2,
			FColor::Green,
			10
		);
		
		FU::Draw::DrawDebugLine(
			GetWorld(),
			Pos2,
			Pos3,
			FColor::Green,
			10
		);
		FU::Draw::DrawDebugLine(
			GetWorld(),
			Pos3,
			Pos1,
			FColor::Green,
			10
		);
	}
	
	return bResult;
}

```

Example results:
![[Pasted image 20260601144702.png]]

![[Pasted image 20260601144721.png]]


## Edit Exported Nav Geo

```c++
bool USANTestStaticMeshComponent::DoCustomNavigableGeometryExport(FNavigableGeometryExport& GeomExport) const
{
	const bool bResult = Super::DoCustomNavigableGeometryExport(GeomExport);
	
	auto& RecastGeoExport = static_cast<FRecastGeometryExport&>(GeomExport);
	
	if (RecastGeoExport.Data)
	{
		RecastGeoExport.Data->Bounds = RecastGeoExport.Data->Bounds.ShiftBy(FVector(0, 0, 100));
	}
	
	const int32 TriangleCount = RecastGeoExport.IndexBuffer.Num() / 3;
	
	// we have to track edited Z values since the VertexBuffer contains unique vertex coords
	TSet<int32> EditedZIndices;
	EditedZIndices.Reserve(RecastGeoExport.IndexBuffer.Num());
	
	auto KeepOrEditZVal = [&EditedZIndices] (int32 ZIndex, double& ZVal)
	{
		if (!EditedZIndices.Contains(ZIndex))
		{
			EditedZIndices.Add(ZIndex);
			ZVal += 100;
		}
	};
	
	for (int32 TriangleIndex = 0; TriangleIndex < TriangleCount; ++TriangleIndex)
	{
		const int32 IndexOffset = TriangleIndex * 3;
		
		const int32 Index1 = RecastGeoExport.IndexBuffer[IndexOffset];
		const int32 Index2 = RecastGeoExport.IndexBuffer[IndexOffset + 1];
		const int32 Index3 = RecastGeoExport.IndexBuffer[IndexOffset + 2];
		
		double& Vertex1X = RecastGeoExport.VertexBuffer[Index1 * 3];
		double& Vertex1Y = RecastGeoExport.VertexBuffer[Index1 * 3 + 1];
		const int32 Vertex1ZIndx = Index1 * 3 + 2;
		double& Vertex1Z = RecastGeoExport.VertexBuffer[Vertex1ZIndx];
		KeepOrEditZVal(Vertex1ZIndx, Vertex1Z);
		
		double& Vertex2X = RecastGeoExport.VertexBuffer[Index2 * 3];
		double& Vertex2Y = RecastGeoExport.VertexBuffer[Index2 * 3 + 1];
		const int32 Vertex2ZIndx = Index2 * 3 + 2;
		double& Vertex2Z = RecastGeoExport.VertexBuffer[Vertex2ZIndx];
		KeepOrEditZVal(Vertex2ZIndx, Vertex2Z);
		
		double& Vertex3X = RecastGeoExport.VertexBuffer[Index3 * 3];
		double& Vertex3Y = RecastGeoExport.VertexBuffer[Index3 * 3 + 1];
		const int32 Vertex3ZIndx = Index3 * 3 + 2;
		double& Vertex3Z = RecastGeoExport.VertexBuffer[Vertex3ZIndx];
		KeepOrEditZVal(Vertex3ZIndx, Vertex3Z);
		
		const FVector Vertex1 = FVector(Vertex1X, Vertex1Y, Vertex1Z);
		const FVector Vertex2 = FVector(Vertex2X, Vertex2Y, Vertex2Z);
		const FVector Vertex3 = FVector(Vertex3X, Vertex3Y, Vertex3Z);
	}
	
	return bResult;
}

```

Example result:

![[Pasted image 20260602004041.png]]
# Generation process
See also [Static NavMesh Generation by Hussein Khalil](https://www.unrealdoc.com/i/132416769/logic-and-data-flow).
