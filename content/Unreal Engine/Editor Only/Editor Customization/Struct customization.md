
# Struct title display name

## About

> [!Warning] Only for array of structs
> 

See [details and example here](https://unreal-garden.com/docs/uproperty/#titleproperty) using the meta attribute `TitleProperty`.
<br>![[Data/Pasted image 20250518171710.png]]

> [!Info] Examples
> - `TitleProperty="MyStructProperty"`
> - `TitleProperty="{MyStructProperty}"`
> - `TitleProperty="Some text and {MyStructProperty}!"`

## In Editor

**Internally the editor seems to use `FTitleMetadataFormatter` to get the final display name.**
`FTitleMetadataFormatter::TryParse` is called which returns an `FTitleMetadataFormatter` that we use to call `GetDisplayText`. (See `SPropertyEditorArrayItem::Construct` or  `FItemPropertyNode::GetDisplayName` for an example of that process).

**So how does this works?** <br>
`FTitleMetadataFormatter` is a really simple struct, it has two member vars: `FText Format` and `TArray<TSharedPtr<IPropertyHandle>> PropertyHandles`. and the 2 functions I mentioned above.

**Example 1 (no formatting)** <br>
Lets take for first example `TitleProperty="PushEndOffset"`. No fancy formatting just the name of a property in my struct which is a `FVector`.
In this example we will imagine we only have one entry in the array (the process is the same for each entry).

Since we have no formatting `FTitleMetadataFormatter::TryParse` will just get the property handle of the property name we gave (`PushEndOffset`) and add it to `PropertyHandles`, `Format` will just be `{PushEndOffset}`.

Then inside `GetDisplayText` we create a temp var `FFormatNamedArguments FormatArgs` and iterate all `PropertyHandles`. For each entry we call `IPropertyHandle::GetValueAsDisplayText` (which should call `FProperty::ExportText_Internal`, in our example this will return `(X=0.000000,Y=0.000000,Z=0.500000)`) and add it to `FormatArgs`.

In the end we call `FText::Format(Format, FormatArgs)` and the result is used as the display text.

**Example 2 (with formatting) *<br>*
*To avoid repetitive content I will just point out the differences, which here are in `FTitleMetadataFormatter::TryParse`*
In this second example we will have `TitleProperty="{PushEndOffset}"`.

If "formatting chars" are detected (editor seems to only check for `{`)  `FText::GetFormatPatternParameters` will be called, which returns an array of all the elements we put inside `{}`.
Then for each element we get property handle from the struct and add it to `PropertyHandles`.
`Format` will simply equal to whatever `TitleProperty` is.

## Little trick for more fancy display name

```c++
// .h

// Our struct
USTRUCT(BlueprintType, DisplayName="Push Data For Tag Query")
struct FPTPushDataForTagQuery
{
	GENERATED_BODY();

	FPTPushDataForTagQuery();
	
	void PostSerialize(const FArchive& Ar);

#if WITH_EDITORONLY_DATA
	// Without an attribute such as EditAnywhere or VisibleAnywhere the editor TitleProperty code won't be able to find this property.
	UPROPERTY(VisibleAnywhere, meta=(EditCondition="false", EditConditionHides))
	FString EditorDisplayName;
#endif

	UPROPERTY(EditAnywhere, Category="PushThemAll|Setup")
	FGameplayTagRequirements TagRequirements;

	UPROPERTY(EditAnywhere, Category="PushThemAll|Setup")
	FVector PushEndOffset;
	
};
template<>
struct TStructOpsTypeTraits<FPTPushDataForTagQuery> : public TStructOpsTypeTraitsBase2<FPTPushDataForTagQuery>
{
	enum
	{
		WithPostSerialize = true,
   };
};


// somewhere in a class
UPROPERTY(EditAnywhere, meta=(TitleProperty="Target: {EditorDisplayName}"))
TArray<FPTPushDataForTagQuery> PushDataForQueries;


// .cpp
FPTPushDataForTagQuery::FPTPushDataForTagQuery():
	PushEndOffset(FVector::ZeroVector)
{
	// We cannot set EditorDisplayName here because when created for editor details panel view the struct will be in default state and not have our user overrides.
}

void FPTPushDataForTagQuery::PostSerialize(const FArchive& Ar)
{
	// Here the struct instance will have user overrides.
	EditorDisplayName = FString::Printf(TEXT("%s"), *TagRequirements.ToString());
}
```
