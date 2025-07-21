
# `UBlueprint`

The `UBlueprint` object is the actual Blueprint asset you have in the editor.
This means that there is one `UBlueprint` instance for each Blueprint asset.

> [!Warning] Warning
> `UBlueprint` is an editor only concept

You can get the `UBlueprint` from an `UClass` by using `ClassGeneratedBy`.

# `UBlueprintGeneratedClass`

Objects ending with `_C` are `UBlueprintGeneratedClass`s

You can see it as the cooked blueprint class (the thing that is being created when you press the BP compile button next to the save one).

You can get it by casting an `UClass` to an `UBlueprintGeneratedClass`


# Other linked resources
- [[UPackage]]
- [[Reflection System]]
- [[Core#Object]]
- [[SCS & Nodes]]


