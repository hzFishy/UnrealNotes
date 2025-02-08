

# Issues

## OnRep OldValue
When using OldValue in `OnRep_` functions, be sure to make your parameter a reference, otherwise your value will be broken.

Example:
```c++
// broken
UFUNCTION() void OnRep_HidingActor(TWeakObjectPtr<ATFCInGameCharacter> OldVal);
// works 
UFUNCTION() void OnRep_HidingActor(TWeakObjectPtr<ATFCInGameCharacter>& OldVal);`
```

