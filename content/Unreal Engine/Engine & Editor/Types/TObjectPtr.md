
Since 5.5 `TObjectPtr<>` is mandatory for `UObject` variables (only for members of classes and structs). You still need to mark the variable with `UPROPERTY()` or the `UObject` ref will never be cleared.

More [info](https://www.thegames.dev/?p=279) for converting between rare ptrs to TObjectPtr (`ObjectPtrDecay`, ``)

In `Target.cs` see also `NativePointerMemberBehaviorOverride` using enum `PointerMemberBehavior`.
