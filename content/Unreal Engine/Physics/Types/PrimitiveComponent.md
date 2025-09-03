# About
It holds a `FBodyInstance BodyInstance` initialized in `UPrimitiveComponent::OnCreatePhysicsState`.
Most (all?) of their physic settings are in `BodyInstance` and `FBodyInstanceCore`.

Some special settings like the constraints are in `BodyInstanceCustomization.cpp` (`FBodyInstanceCustomizationHelper`).

For **Constraint** details check [[Body Instance]] and [[Constraints]]

# Classes
```mermaid
flowchart TD
%%{ init : {'flowchart': {'nodeSpacing': 100, 'rankSpacing': 100, "curve" : "step"}} }%%

    UMeshComponent --> UPrimitiveComponent
    UProceduralMeshComponent --> UMeshComponent
    UMeshComponent --> UPrimitiveComponent
    USkinnedMeshComponent --> UMeshComponent
    UWidgetComponent --> UMeshComponent
    UBaseDynamicMeshComponent --> UMeshComponent
    UDynamicMeshComponent --> UBaseDynamicMeshComponent
    UGeometryCollectionComponent --> UMeshComponent
    UStaticMeshComponent --> UMeshComponent
    USplineMeshComponent --> UStaticMeshComponent
    USkeletalMeshComponent --> USkinnedMeshComponent
    UPoseableMeshComponent --> USkinnedMeshComponent
    UInstancedSkinnedMeshComponent --> USkinnedMeshComponent
    UInstancedStaticMeshComponent --> UStaticMeshComponent
```