Snippets taken from [here](https://1danielcoelho.github.io/unreal-engine-basics-base-classes/) (if link is broken use [this](https://github.com/1danielcoelho/1danielcoelho.github.io/blob/main/_posts/2021-02-14-unreal-engine-basics-base-classes.md))

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

```cpp
UClass* MyObjectsClass = UMyObject::StaticClass();

for (TFieldIterator<FProperty> PropertyIterator( MyObjectsClass); PropertyIterator; ++PropertyIterator )
{
    FProperty* Property = *PropertyIterator;

    // Would log "MyValue" and "MyValueArray"
    UE_LOG( LogTemp, Log, TEXT( "%s" ), *Property->GetName() );
}

for (TFieldIterator<UFunction> FunctionIterator( MyObjectsClass); FunctionIterator; ++FunctionIterator)
{
    UFunction* Func = *FunctionIterator;

    // Would log Multiply
    UE_LOG(LogTemp, Log, TEXT( "%s" ), *FunctionIterator->GetName());
}
```
