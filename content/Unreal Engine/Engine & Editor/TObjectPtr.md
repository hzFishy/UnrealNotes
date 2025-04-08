
Since 5.5 `TObjectPtr<>` is mandatory for UObject variables (only for members of classes and structs)

You still need to mark the variable with `UPROPERTY()` or the UObject ref will never be cleared.