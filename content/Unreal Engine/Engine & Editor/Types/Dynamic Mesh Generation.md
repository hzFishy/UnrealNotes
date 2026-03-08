
About `UDynamicMeshComponent`, `UDynamicMesh` and `FDynamicMesh3`.

# `UDynamicMeshComponent`
`UDynamicMeshComponent` renders a `UDynamicMesh`.
See `UDynamicMeshComponent::SetDynamicMesh`.

# `UDynamicMesh`
An `UDynamicMesh` is a wrapper for a `FDynamicMesh3`.
See `UDynamicMesh::SetMesh`.
See `UDynamicMesh::ResetToCube` for an example on how you can use `UDynamicMesh::EditMeshInternal` to append shapes and do other actions. 

# `FMeshShapeGenerator`
This and its subclasses can be used to generate a `FDynamicMesh3`.

See the `FDynamicMesh3::FDynamicMesh3(const FMeshShapeGenerator* Generator)` constructor.
This allows to do this like:
```c++
FMinimalBoxMeshGenerator BoxGen;
BoxGen.Box = UE::Geometry::FOrientedBox3d(FVector3d::Zero(), 50.0 * FVector3d::One());
FDynamicMesh3 EditMesh = FDynamicMesh3(&BoxGen.Generate());
```
