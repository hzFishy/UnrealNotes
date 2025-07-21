Basically, for whatever assets the default picker finds, it loops them with your function.

The function must have the following signature: `UFUNCTION() bool FuncName(const FAssetData& AssetData) const;` But it seems that non const functions works to.

**If you return true you are excluding the given Asset Data**
# Simple use case
Snippet (thanks to KaosSpectrum)
```c++
// add this to your UPROPERTY
meta = (GetAssetFilter="MyOwnFilter")
bool UMyBlah::MyOwnFilter(const FAssetData& AssetData)
{
    //Do asset checking here
}
```

# Filtering Data Tables from base struct
You can filter `UDataTable` pickers by using `RowType=XXX` to only display data tables that use a specific row type.

If you want more control on this (for example here, including all struct childs from a base struct), you can use `GetAssetFilter`.

Snippet (thanks to KaosSpectrum)
```c++
// Lets we got these 2 structs
USTRUCT(BlueprintType)
struct FTestParentRow : public FTableRowBase { GENERATED_BODY(); };
USTRUCT(BlueprintType)
struct FChildParentRow : public FTestParentRow { GENERATED_BODY(); };

// the property
UPROPERTY(EditAnywhere, Category="Data", meta=(GetAssetFilter="MyTableRowsFilter"))
TObjectPtr<UDataTable> MyDataTable;

// the filter
bool UMyObject::MyTableFowsFilter(const FAssetData& AssetData)
{
    FString Parent = FTestParentRow::StaticStruct()->GetPathName();
    static const FName RowStructureTagName("RowStructure");
    FString RowStructure;
    if (AssetData.GetTagValue<FString>(RowStructureTagName, RowStructure))
    {
        if (RowStructure == Parent)
        {
            return false;
        }
        
        // This is slow, but at the moment we don't have an alternative to the short struct name search
        UScriptStruct* RowStruct = UClass::TryFindTypeSlow<UScriptStruct>(RowStructure);
        if (RowStruct && RowStruct->IsChildOf(FTestParentRow::StaticStruct()))
        {
            return false;
        }
    }
    return true;
}
```
