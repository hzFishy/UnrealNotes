
The CMC is placed in [[Networking]] because most of the important parts and difficulties occurs when you need replication and prediction.


**Resources**
- [How the CMC works under the hood](https://www.youtube.com/watch?v=urkLwpnAjO0&list=PLXJlkahwiwPmeABEhjwIALvxRSZkzoQpk)
- [UE Custom CMC Network Data](https://docs.google.com/document/d/1UO6Ww6Lfpti3YElVdo9uioTUtQJQ9CoSLvd9kF8hvJo/edit?usp=sharing)

**Optimizing**
- [Character Movement Component Optimizations](https://dev.epicgames.com/community/learning/knowledge-base/mo9O/unreal-engine-character-movement-optimizations)


# Partial Breakdown

When walking, when you move (or tick?) CMC will eventually run the following:
- `StartNewPhysics` calls `PhysWalking` (only call of `PhysWalking`)
	- `PhysWalking` calls `MoveAlongFloor`
		- `MoveAlongFloor` calls `SafeMoveUpdatedComponent`
			- `SafeMoveUpdatedComponent` will run checks and eventually call `MoveUpdatedComponentImpl`, which calls `MoveComponent` on `UpdatedComponent` (by default it is the root, which is the capsule component)
		- Then, here it checks if we are stuck or not. And if its a another rampe (using Normal hit) or wall/barrier.