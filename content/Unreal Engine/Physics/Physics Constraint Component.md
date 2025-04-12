
> [!Danger] The child is `Component/Bone 1` and the parent is `Component/Bone 2` !!


## Stability
To have a stable chain of physics constraints (eg: multiple capsules forming a cable) you should have everything parent correctly.

For example, if you have a cable, a player capsule and a static cube, the chain should be `Cube -> Cable Capsule -> ... -> Cable Capsule -> Player Capsule` as `Parent -> Child`.

> [!Quote] Adriel
> *"The solver will iterate trying to solve the chain, then after that it'll do the projection step to pull every child towards its parent.
> You want everything pulled towards the solved state which is a chain of things from the cube to the player"*

You can (and should) try to bump the number in the physics solver settings (in Project Settings). <br>
![[Pasted image 20250411214537.png]]
