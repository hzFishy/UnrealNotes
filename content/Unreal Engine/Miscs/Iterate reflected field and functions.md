Snippets taken from [here](https://1danielcoelho.github.io/unreal-engine-basics-base-classes/) (if link is broken use [this](https://github.com/1danielcoelho/1danielcoelho.github.io/blob/main/_posts/2021-02-14-unreal-engine-basics-base-classes.md))

# Example

**Dummy class**
```c++
UCLASS()
class UMyObject : public UObject
{
    GENERATED_BODY()

public:
    UFUNCTION(BlueprintCallable)
    float Multiply(float OtherValue)
    {
        return MyValue * OtherValue;
    }

    UPROPERTY(EditAnywhere)
    float MyValue = 2.3f;

    UPROPERTY(EditAnywhere)
    TArray<int32> MyValueArray;

    float UnAnnotatedValue = 3.0f;
};
```


**Iterate the fields and functions**
```cpp
UClass* MyObjectsClass = UMyObject::StaticClass();

for (TFieldIterator<FProperty> PropertyIterator(MyObjectsClass); PropertyIterator; ++PropertyIterator)
{
    FProperty* Property = *PropertyIterator;

    // Would log "MyValue" and "MyValueArray"
    UE_LOG( LogTemp, Log, TEXT( "%s" ), *Property->GetName() );
}

for (TFieldIterator<UFunction> FunctionIterator(MyObjectsClass); FunctionIterator; ++FunctionIterator)
{
    UFunction* Func = *FunctionIterator;

    // Would log Multiply
    UE_LOG(LogTemp, Log, TEXT( "%s" ), *FunctionIterator->GetName());
}
```

**Get value**
> For nesting support see [this](https://github.com/EpicGames/UnrealEngine/blob/803688920e030c9a86c3659ac986030fba963833/Engine/Source/Runtime/Engine/Private/PrintObjectUtils.cpp#L122)

```c++
for (TFieldIterator<FProperty> PropertyIterator(PrefabPropertiesObjectClass); PropertyIterator; ++PropertyIterator)
{
	FProperty* Property = *PropertyIterator;
	// example of skip
    if (auto* DelegateProperty = CastField<FMulticastDelegateProperty>(Property))  
    {
	    continue;
    }

    void* Owner = PrefabPropsObject.Get();
    void* PropertyData = Property->ContainerPtrToValuePtr<void>(Owner);
    
    FString ExportedTextString;  
    Property->ExportText_Direct(ExportedTextString, PropertyData, PropertyData, nullptr, PPF_DebugDump);
    ReflectedProperties.Emplace(FString::Printf(TEXT("%s: %s"), *Property->GetName(), *ExportedTextString));
}
```

# `TFieldIterator`
You can give a `TFieldIterator` in the `TFieldIterator` constructor for more customization like `IncludeSuper`

# Functions

## Flags
You can get the reflected function flags by reading `FunctionFlags`.
For example, to check if a function is an RPC you should do `if (Function->FunctionFlags & (FUNC_NetServer | FUNC_NetClient | FUNC_NetMulticast))`

