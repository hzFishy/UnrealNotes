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


For more debug draw methods, see `ChaosDebugDraw.cpp` file
