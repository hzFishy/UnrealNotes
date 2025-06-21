

- `ClassPrivate` Is the `UClass` of the object instance. The thing you get back when you call `GetClass()`.
- If a object was loaded from "a loader" (`FAsyncPackage` or `LoadPackageInternal`) it will have the `RF_WasLoaded` flag. This can be very useful to use on actors to know if they got loaded from disk or spawned at runtime. 
- [UObject Construction & Post Initalization](https://github.com/staticJPL/Unreal-Engine-Documentation/blob/6fb6beee4484bd4b15cf037dbccafc823e21457b/Unreal%20Engine%20UObject%20Construction%20%26%20Post%20Initalization/Main.md)


# Reflection
Since 5.5 `TObjectPtr<>` is mandatory for `UObject` variables (only for members of classes and structs). You still need to mark the variable with `UPROPERTY()` or the `UObject` ref will never be cleared.

See
- [[Reflection System]]
- [[Garbage Collection]]

There is also `AddReferencedObjects`, for example this is used in AActor to add all owned components.
The GC will call this as much as it needs (`TStrongObjectPtr` would be better performance wise in some cases).

> [!Warning] To use `TStrongObjectPtr` you must include the type


# Archetype
`GetArchetype` gives you the template object that was used.
For example, the UObject you get from a BP editor sub object editor (with `BlueprintEditor->GetSubobjectEditor()->GetDragDropTree()->GetSelectedItems()`) is the same than the returned archetype of the component that lives in the preview world.


# GEN_VARIABLE
These objects are templates, meaning they aren't owned by an actor component instance but by the supposed owning actor class (get it by using `GetTypedOuter<UClass>()`)

# Virtual functions details
- [Actor Load/Init Function Cheatsheet](https://ikrima.dev/ue4guide/gameplay-programming/actor-tick-lifecycle-flow/actor-lifecycle-diagram/)

- `PostTransacted` is called when you move/edit anything in a BP
- When you compile a BP, `PostInitProperties`, `PostEditChangeProperty` and `PostCDOContruct` are called.
- When you edit a property `PostEditChangeProperty` is called (Doesn't work for Transforms)
- `PostCDOContruct` always has its World null.


# TStrongObjectPtr
Use `MyStrPtr.Reset(SomeObject)` to set it.
