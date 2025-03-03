## Profiles
Profiles can be created in a unlimited amount, so be free to use it at anytime a specific "thing" with a specific set of collisions rules exists more than once.

## Collision rules
The Engine takes the lowest collision rule.

> [!Info]- Example
> If i got 2 object types, ObjectA and ObjectB. ObjectA wants to block ObjectB, but ObjectB ignores ObjectA : then both can overlap, because Ignore is lower than Block.

## Custom ignore
Like traces you can give a list of actors/components to ignore to any `UPrimitiveComponent`. There is also a custom mask system.


## `CollisionEnabled` in depth
From what I've found in the source code, there **is** an actual performance change depending on `TEnumAsByte<ECollisionEnabled::Type> CollisionEnabled;`.
Full details:
- **No Collision**:
	- Will not create any representation in the physics engine. Cannot be used for spatial queries (raycasts, sweeps, overlaps) or simulation (rigid body, constraints). 
	- Best performance possible (especially for moving objects)  
- **Query Only**:
	- Only used for spatial queries (raycasts, sweeps, and overlaps). Cannot be used for simulation (rigid body, constraints). Useful for character movement and things that do not need physical simulation.
	- Performance gains by keeping data out of simulation tree.  
- **Physics Only**:
	- Only used only for physics simulation (rigid body, constraints). Cannot be used for spatial queries (raycasts, sweeps, overlaps). Useful for jiggly bits on characters that do not need per bone detection.
	- Performance gains by keeping data out of query tree  
- **Collision Enabled**:
	- Can be used for both spatial queries (raycasts, sweeps, overlaps) and simulation (rigid body, constraints).  

# Resources & tips
- [Collision Data in UE5: Practical Tips for Managing Collision Settings & Queries | Unreal Fest 2023](https://www.youtube.com/watch?v=xIQI6nXFygA)
