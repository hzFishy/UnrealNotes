
# Creation

## Do not subclass `SWidget`
As mentioned in the class comment, do not subclass directly `SWidget`.
Epic recommends using `LeafWidget` or `SPanel` (or any other subclass) as the minimum slate widget class.

## Example of custom Slate class with args
```c++
// .h
class SPSPrefabSystemBlueprintWindow : public SVerticalBox
{
	SLATE_BEGIN_ARGS(SPSPrefabSystemBlueprintWindow)
		: _BlueprintEditorPtr(),
		_PrefabEditorSubsystem()
    {}
	
    SLATE_ARGUMENT(TSharedPtr<FBlueprintEditor>, BlueprintEditorPtr)
    SLATE_ARGUMENT(UPSPrefabEditorSubsystem*, PrefabEditorSubsystem)
	
    SLATE_END_ARGS()
	
public:
    SPSPrefabSystemBlueprintWindow(): PrefabEditorSubsystem(nullptr) {}
	
    void Construct(const FArguments& InArgs);
	
    virtual void Tick(const FGeometry& AllottedGeometry, const double InCurrentTime, const float InDeltaTime) override;
	
    virtual int32 OnPaint(const FPaintArgs& Args, const FGeometry& AllottedGeometry, const FSlateRect& MyCullingRect, FSlateWindowElementList& OutDrawElements, int32 LayerId, const FWidgetStyle& InWidgetStyle, bool bParentEnabled) const override;
	
private:
    TSharedPtr<FBlueprintEditor> BlueprintEditorPtr;
    UPSPrefabEditorSubsystem* PrefabEditorSubsystem;
};

// .cpp
void SPSPrefabSystemBlueprintWindow::Construct(const FArguments& InArgs)
{
	BlueprintEditorPtr = InArgs._BlueprintEditorPtr;
    PrefabEditorSubsystem = InArgs._PrefabEditorSubsystem;
    
    const bool bHasBlueprintPRC = PrefabEditorSubsystem->DoesBlueprintHavePRC(BlueprintEditorPtr.Get());
    if (bHasBlueprintPRC)
    {
	    AddSlot()
		    .AutoHeight()
		    [
			    SNew(STextBlock).Text(INVTEXT("Hello"))
			];
    }
    else
    {
	    AddSlot()
		    .AutoHeight()
		    [
			    SNew(STextBlock).Text(INVTEXT("No Prefab Reference Component were found in this Blueprint Actor.")).ColorAndOpacity(FSlateColor(FColor::Red))
			];
    }
}
    
void SPSPrefabSystemBlueprintWindow::Tick(const FGeometry& AllottedGeometry, const double InCurrentTime, const float InDeltaTime)  
{
	SWidget::Tick(AllottedGeometry, InCurrentTime, InDeltaTime);  
}
	
int32 SPSPrefabSystemBlueprintWindow::OnPaint(const FPaintArgs& Args, const FGeometry& AllottedGeometry, const FSlateRect& MyCullingRect, FSlateWindowElementList& OutDrawElements, int32 LayerId, const FWidgetStyle& InWidgetStyle, bool bParentEnabled) const
{
	return SVerticalBox::OnPaint(Args, AllottedGeometry, MyCullingRect, OutDrawElements, LayerId, InWidgetStyle, bParentEnabled);
}
	
```

## Assign new widget to variable for later edit

```c++
TSharedPtr<SHorizontalBox> HorizontalBox;
// ...
SAssignNew(HorizontalBox, SHorizontalBox)
// ...
```


## Combo box
See example of use of `SComboBox`

## Horizontal & Vertical Box

Examples
```c++
// Method 1
SNew(SHorizontalBox)
+SHorizontalBox::Slot()
.AutoWidth()
[
    SNew(STextBlock).Text(INVTEXT("Hello"))
]
+SHorizontalBox::Slot()
.AutoWidth()
[
    SNew(STextBlock).Text(INVTEXT("Hello"))
]

// method 2
TSharedPtr<SVerticalBox> VerticalBox;  
SAssignNew(VerticalBox, SVerticalBox);  
if (bSomeCondition)  
{  
    VerticalBox->AddSlot()  
       .AutoHeight()
       [
	       SNew(STextBlock).Text(INVTEXT("Hello"))
	   ];  
}
else
{
	VerticalBox->AddSlot()
		.AutoHeight()
		[
			SNew(STextBlock).Text(INVTEXT("No Prefab Reference Component were found in this Blueprint Actor.")).ColorAndOpacity(FSlateColor(FColor::Red))
		];
}
```

## Fill dropdown from enum
See `SEnumComboBox`

Example:
```c++
auto PreviewSelectionDrawModeWidget = SNew(SEnumComboBox, StaticEnum<EPSEditorPRCPreviewSettings_SelectionDrawMode>())
.OnEnumSelectionChanged(SEnumComboBox::FOnEnumSelectionChanged::CreateLambda([this, BlueprintEditorContext] (int32 InValue, ESelectInfo::Type InSelectInfo)
{...}))
.CurrentValue_Lambda([this, BlueprintEditorContext]()  
{ return ...});

```

## Common Slate Arguments
[Common Slate Arguments (Docs)](https://dev.epicgames.com/documentation/en-us/unreal-engine/slate-ui-widget-examples-for-unreal-engine)

## Scroll Box
Use `SScrollBox`.

## Color Block
Use ` SColorBlock`. Good for lines to