
# Classes

## `UBlueprintAsyncActionBase`
Base class for async actions.

> BlueprintCallable factory functions for classes which inherit from UBlueprintAsyncActionBase will have a special blueprint node created for it: UK2Node_AsyncAction.
> You can stop this node spawning and create a more specific one by adding the UCLAS metadata "HasDedicatedAsyncNode"

## `UCancellableAsyncAction`
Use this if you async action can be aborted.

> Base class for asynchronous actions that can be spawned from UK2Node_AsyncAction or C++ code.
> These actions register themselves with the game instance and need to be explicitly canceled or ended normally to go away.
> The ExposedAsyncProxy metadata specifies the blueprint node will return this object for later canceling.


