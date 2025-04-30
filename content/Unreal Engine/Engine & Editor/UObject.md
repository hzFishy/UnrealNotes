

- `ClassPrivate` Is the `UClass` of the object instance. The thing you get back when you call `GetClass()`.
- If a object was loaded from "a loader" (`FAsyncPackage` or `LoadPackageInternal`) it will have the `RF_WasLoaded` flag. This can be very useful to use on actors to know if they got loaded from disk or spawned at runtime. 
- [UObject Construction & Post Initalization](https://github.com/staticJPL/Unreal-Engine-Documentation/blob/6fb6beee4484bd4b15cf037dbccafc823e21457b/Unreal%20Engine%20UObject%20Construction%20%26%20Post%20Initalization/Main.md)


# Reflection
Since 5.5 `TObjectPtr<>` is mandatory for UObject variables (only for members of classes and structs). You still need to mark the variable with `UPROPERTY()` or the UObject ref will never be cleared.

- [[Reflection System]]
- [[Garbage Collection]]

There is also `AddReferencedObjects`, for example this is used in AActor to add all owned components.
The GC will call this as much as it needs (`TStrongObjectPtr` would be better performance wise in some cases).

> [!Warning] To use `TStrongObjectPtr` you must include the type





# Virtual details
- [Actor Load/Init Function Cheatsheet](https://ikrima.dev/ue4guide/gameplay-programming/actor-tick-lifecycle-flow/actor-lifecycle-diagram/)

- `PostTransacted` is called when you move/edit anything in a BP
- When you compile a BP, `PostInitProperties`, `PostEditChangeProperty` and `PostCDOContruct` are called.
- When you edit a property `PostEditChangeProperty` is called (Doesn't work for Transforms)
- `PostCDOContruct` always has its World null.