
# Physics Constraint Component

> [!Danger] The child is `Component/Bone 1` and the parent is `Component/Bone 2` !!

## Stability
To have a stable chain of physics constraints (eg: multiple capsules forming a cable) you should have everything parent correctly.

For example, if you have a cable, a player capsule and a static cube, the chain should be `Cube -> Cable Capsule -> ... -> Cable Capsule -> Player Capsule` as `Parent -> Child`.

> [!Quote] Adriel
> *"The solver will iterate trying to solve the chain, then after that it'll do the projection step to pull every child towards its parent.
> You want everything pulled towards the solved state which is a chain of things from the cube to the player"*

You can (and should) try to bump the number in the physics solver settings (in Project Settings). <br>
![[Pasted image 20250411214537.png]]

## Breaking

Before calling `OnConstraintBroken` inside `UPhysicsConstraintComponent::OnConstraintBrokenWrapper` (there is also `UPhysicsConstraintComponent::OnConstraintBrokenHandler`) the breaking state is pulled in `FPhysicsSolverBase::PullPhysicsStateForEachDirtyProxy_External` in a loop iterating `LatestData->DirtyJointConstraints` and calling `FJointConstraintPhysicsProxy::PullFromPhysicsState` on each entry.

Inside `FPhysScene_Chaos::OnSyncBodies`, if the `bIsBreaking` in `FJointConstraint, FOutputData` is true the `FConstraintBrokenDelegateWrapper` wrapper will be called (see `OnConstraintBrokenWrapper` above).

> [!Note]- About `bIsBroken` in OutputData
> It reflects if `IsConstraintEnabled` return false (`bIsBroken = !IsConstraintEnabled`)

This output data is pulled and set in `FJointConstraintPhysicsProxy::PullFromPhysicsState` (see above) by reading the buffer `FDirtyJointConstraintData`.

The actual *set* of `bIsBreaking` (and the rest of the `FDirtyJointConstraintData` Buffer) is inside `FJointConstraintPhysicsProxy::BufferPhysicsResults` where it calls `IsConstraintBreaking` on a `FPBDJointConstraintHandle` (`FPhysScene_Chaos::OnSyncBodies`).

This calls `FPBDJointConstraints::IsConstraintBreaking` on a `FConstraintContainer` (templated to `FPBDJointConstraints` for `FPBDJointConstraintHandle`), which just reads the value of `bBreaking` on the element stored in `ConstraintStates`.

This `bBreaking` state is set by `FPBDJointConstraints::SetConstraintBroken` (only called by `FPBDJointConstraints::BreakConstraint` *(by the way this also disables the constraint)* and `FixConstraint`).
The only usage of this break function is `FPBDJointConstraints::SetSolverResults` which is called in `Chaos::Private::ScatterOutputImpl` (called by `FPBDJointContainerSolver::ScatterOutput`).

This function will set the value of the argument `bIsBroken` by calling `IsBroken()` on the solver (templated to `FPBDJointCachedSolver` here). This function just returns the value of the variable `bIsBroken` of the cached solver.
This variable is only set by `SetIsBroken()` which is only called in `Chaos::Private::ApplyPositionConstraintsImpl` (called by `FPBDJointContainerSolver::ApplyPositionConstraints`, search for `Solver.SetIsBroken`).

This `Solver.SetIsBroken` call occurs because `GetJointShouldBreak` returned true.
Since `GetJointShouldBreak` is called on a `FPBDJointCachedSolver`  its doing some math to get the linear and angular impulse so you can't simply read some variables to get the values.

> [!Note]- Debug values of `LinearImpulse` and `AngularImpulse` in `GetJointShouldBreak`
> (Example for `LinearImpulse`)
> - Add a conditional breakpoint by using `LinearForceSq > LinearThresholdSq` at the last line of the first if block
> - To get the value of `LinearForceSq` run the following `(LinearImpulse.X*LinearImpulse.X + LinearImpulse.Y*LinearImpulse.Y + LinearImpulse.Z*LinearImpulse.Z) / (Dt * Dt * Dt * Dt)`. This mimics the math used to get `LinearForceSq`.


## Motor
Orientation, Position and Velocity Target of the motors are relative to the constraint origin.

# Constraint on body instance
See [[Body Instance#Constraint]]

# Commands & debug
Some interesting commands, a lot more available
- `Stat ChaosConstraintSolver`
- `Stat ChaosConstraintDetails`

See [[Console commands & debugging|Console commands & debugging]] in `/Physics` for more Chaos commands