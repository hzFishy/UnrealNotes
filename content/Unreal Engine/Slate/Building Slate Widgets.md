
# Miscs
## Creation
### Assign new widget to variable for later edit
```c++
TSharedPtr<SHorizontalBox> HorizontalBox;
// ...
SAssignNew(HorizontalBox, SHorizontalBox)
// ...
```


### Make combo box
See example of use of `SComboBox`

### Horizontal Box
Example
```c++
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
```

### Fill dropdown from enum
See `SEnumComboBox`

Example:
```c++
auto PreviewSelectionDrawModeWidget = SNew(SEnumComboBox, StaticEnum<EPSEditorPRCPreviewSettings_SelectionDrawMode>())
.OnEnumSelectionChanged(SEnumComboBox::FOnEnumSelectionChanged::CreateLambda([this, BlueprintEditorContext] (int32 InValue, ESelectInfo::Type InSelectInfo)
{...}))
.CurrentValue_Lambda([this, BlueprintEditorContext]()  
{ return ...});

```

### Common Slate Arguments
[Common Slate Arguments (Docs)](https://dev.epicgames.com/documentation/en-us/unreal-engine/slate-ui-widget-examples-for-unreal-engine)