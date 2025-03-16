
# Body Instance
The body instance seems to be mostly used from `UPrimitiveComponent`.
It also seems that a lot of physic functions are running from `FBodyInstance`.

Some of these functions will register a physic command using `ApplyAsyncPhysicsCommand`, that has a function call-back that's using `FPhysicsInterface`.

- **Change collision:** For some reason`SetCollisionEnabled(NewState)` doesn't work as expected, use `SetShapeCollisionEnabled(0, NewState)` instead.