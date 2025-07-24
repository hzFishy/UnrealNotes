# Resources
- See `PushModel.h`

# Usage
For a same replicate object, you can use the push model for some replicated variables and the regular UE workflow for other variables (just be sure to set the value of `bIsPushBased` to the desired one before declaring vars in `GetLifetimeReplicatedProps`).
## Replicate

In `GetLifetimeReplicatedProps` you must use a special struct
```c++
FDoRepLifetimeParams Params;
Params.bIsPushBased = true;
DOREPLIFETIME_WITH_PARAMS_FAST(AMyAwesomeActor, bMyReplicatedBool, Params);
...
```

Use `MARK_PROPERTY_DIRTY` macros to mark a property as being dirty so it can be picked up to be replicated.

