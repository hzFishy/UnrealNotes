- [Chaos Visual Debugger Overview (Docs)](https://dev.epicgames.com/documentation/en-us/unreal-engine/chaos-visual-debugger-overview-in-unreal-engine)
- [Chaos Visual Debugger - User Guide for UE 5.4](https://dev.epicgames.com/community/learning/tutorials/EpnO/unreal-engine-chaos-visual-debugger-user-guide-for-ue-5-4)

**Console commands**
- `stat Chaos`
- `p.Chaos.DebugDraw.Enabled 1`
	- **Must be called so other commands work**
- `p.Chaos.Solver.DebugDrawShapes 1` (engine will call `FPBDRigidsSolver::DebugDrawShapes` each frame)
	- Red: Not simulated, Static
	- Blue: Not simulated, Movable
	- Yellow: Simulated (Active)
	- Gray: Simulated (Sleeping)
- `p.Chaos.DebugDraw.MaxLines` (default to 20000)
- `p.Chaos.Solver.DebugDraw.Cluster.Constraints 1`
	- For GCC

For more debug draw methods, see `ChaosDebugDraw.cpp` file

# Draw

Chaos has a special helper for drawing, which is thread safe (under the hood it uses the global draw helpers functions, see `DebugDrawChaosCommand`).

See `Chaos::FDebugDrawQueue` and `UChaosDebugDrawComponent`.
Example: `Chaos::FDebugDrawQueue::GetInstance().DrawDebugSphere(ParticlePoint, 4, 32, FColor::Yellow);`

For `p.Chaos.Solver.DebugDrawShapes` the drawing root path is `FPBDRigidsSolver::PostTickDebugDraw` -> `FChaosDDParticle::DrawShapes`.
See also `FChaosDDParticleShape::Draw`.
`FChaosDDParticleShape::GetRenderColor` is used if no color is given (default case for engine) and it has multiple checks to know what color to use, most of the case this falls to reading to use `FChaosDebugDrawColorsByState::GetColorFromState` on `DebugDraw::FChaosDebugDrawSettings`.

**This is done outside the game thread.**