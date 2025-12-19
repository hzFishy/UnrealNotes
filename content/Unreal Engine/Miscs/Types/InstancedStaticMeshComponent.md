
> [!Warning] 
> For some reason most ISMC functions are implemented in InstancedStaticMesh.cpp and not InstancedStaticMeshComponent.cpp

# Physics Init
- If a Physics State was need it will be created, this will then run `UInstancedStaticMeshComponent::CreateAllInstanceBodies` which creates a new BI for each already existing ISMC mesh instance.

# Creation
- `UInstancedStaticMeshComponent::AddInstance`
- `UInstancedStaticMeshComponent::AddInstanceInternal`
	- Create a new id
	- Calls SetupNewInstanceData
	- ...
- `UInstancedStaticMeshComponent::SetupNewInstanceData`
	- This can create a new `FBodyInstance` for the new ISMC mesh instance using the `new` keyword and doing some setup.
	- Calls `UInstancedStaticMeshComponent::InitInstanceBody`


# Removing
- `UInstancedStaticMeshComponent::RemoveInstance`
- `UInstancedStaticMeshComponent::RemoveInstanceInternal`
	- This will destroy the existing BI (if any) for the given ISMC mesh instance by using the `delete` keyword and calling `TermBody`.
