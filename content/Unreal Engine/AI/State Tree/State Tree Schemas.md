
# `UStateTreeSchema`
Schema describing which inputs, evaluators, and tasks a StateTree can contain.
Each StateTree asset saves the schema class name in asset data tags, which can be used to limit which StateTree assets can be selected per use case.

```c++
UPROPERTY(EditDefaultsOnly, Category = AI, meta=(RequiredAssetDataTags="Schema=StateTreeSchema_SupaDupa"))
UStateTree* StateTree;
```

# `UStateTreeComponentSchema`
 StateTree for Actors with StateTree component.
 
# `UStateTreeAIComponentSchema`
Use it on the `UStateTreeAIComponent`, which is placed on a `AIController`.

# `UCameraDirectorStateTreeSchema`
The schema of the StateTree for a StateTree camera director.
