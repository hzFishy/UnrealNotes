
# Reserved function names and reflection
*Thanks to PraetorBlue on the Unity discord for clarifying things*

## Basics
The reserved functions such as `Start`, `Update`, `OnTriggerEnter` and so aren't virtual. Unity call them by using reflection.
The reflection is done at compile time in the case of IL2CPP, but at runtime in Mono, the system saves the MethodInfo for later use.

> [!Info] Info
> It doesn't matter how many copies of a script there is in a scene, the method reflection info only takes memory once.


## Physics events

> [!Quote] PraetorBlue
> *"The physics engine doesn't know anything about monobehacviours etc. the game engine has a mapping beteween physics object IDs and UnityEngine Instance IDs. when it gets notifications about collisions (and so on) from the physics engine it looks up the corresponding Unity objects via instance ID then uses its internal SendMessage system to call the function. The messaging system is the thing that cares about methods existing or not on MonoBehaviours"*

