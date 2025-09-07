
# Miscs
If `Simulate physics` is grayed out it means you are missing a proper collision or a physic asset.

# Physics
> [!Info]
> - You can some physics related code in `SkeletalMeshComponentPhysics.cpp`
> - You can find physics related animation code in `PhysAnim.cpp`.

## How the animation updates the physics

> [!Warning] 
> The following process is done if the `KinematicBonesUpdateType` variable is set to `SkipSimulatingBones`, even if no animations are played.

Inside `USkeletalMeshComponent::PostAnimEvaluation`, after the animations were processed we call `USkeletalMeshComponent::UpdateKinematicBonesToAnim`.
Then if `bEnablePerPolyCollision` is false we will use `FPhysicsCommand::ExecuteWrite` to update all the bodies of the SKM.
If not teleporting it will call`FPhysScene_Chaos::SetKinematicTarget_AssumesLocked` otherwise it will use `FPhysScene_Chaos::SetGlobalPose_AssumesLocked`.
