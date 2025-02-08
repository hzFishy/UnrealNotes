# Profiles
Profiles can be created in a unlimited amount, so be free to use it at anytime a specific "thing" with a specific set of collisions rules exists more than once.

# Collision rules
The Engine takes the lowest collision rule.

> [!Info]- Example
> If i got 2 object types, ObjectA and ObjectB. ObjectA wants to block ObjectB, but ObjectB ignores ObjectA : then both can overlap, because Ignore is lower than Block.

# Custom ignore
Like traces you can give a list of actors/components to ignore to any `UPrimitiveComponent`. There is also a custom mask system.

# Resources & tips
- [Collision Data in UE5: Practical Tips for Managing Collision Settings & Queries | Unreal Fest 2023](https://www.youtube.com/watch?v=xIQI6nXFygA)

> [!tip] Some small optimization tip
> When you set a channel/object type to be IGNORE/OVERLAP/BLOCK this will change how the underlying physic system will register and by doing so, increase the amount of checks (and of course a lot of other stuff).
> If you set something to be IGNORED, you will save some performance (how much? I don't know)

