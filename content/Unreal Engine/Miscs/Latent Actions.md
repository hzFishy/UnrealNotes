
`MoveComponentTo` is declared at `UKismetSystemLibrary::MoveComponentTo`.

The action is updated in `FLatentActionManager::ProcessLatentActions`, in `FLatentActionManager::TickLatentActionForObject` and finally in the virtual function `UpdateOperation` (in `FPendingLatentAction`).

Callback is called here: `CallbackTarget->ProcessEvent(...);` (in `TickLatentActionForObject`)

## MoveComponentTo
The action struct is `FInterpolateComponentToAction`