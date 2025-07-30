
# Resources
- [Zomg's post](https://zomgmoz.tv/unreal/Editor-customization/Creating-custom-property-editors-for-structs-and-other-types)
- [Docs](https://dev.epicgames.com/documentation/en-us/unreal-engine/details-panel-customizations-in-unreal-engine#ipropertytypecustomization)

Difference between `CustomizeHeader` and `CustomizeChildren`
<br>![[Pasted image 20250607233142.png]]
# Miscs

## `PropertyHandle`

`PropertyHandle` in `CustomizeHeader` and `CustomizeChildren` is always the root property (for example, your struct).

## Get/Set property
This works if the property has a `Edit` specifier
```c++
TSharedPtr<IPropertyHandle> SelectedSocketProp = PropertyHandle->GetChildHandle(GET_MEMBER_NAME_CHECKED(FFUPickerSockets, SelectedSocket));  

// Get
FName SelectedSocket;  
SelectedSocketProp->GetValue(SelectedSocket);

// Set 
SelectedSocketProp->SetValue(FName("NewValue"));
```

## `CustomizeChildren`

This will construct default child members for the member props of your type details customization.
```c++
// Thanks to aquanox for the snippet
void FCustomization::CustomizeChildren(TSharedRef<IPropertyHandle> PropertyHandle, IDetailChildrenBuilder& ChildBuilder, IPropertyTypeCustomizationUtils& CustomizationUtils)  
{  
    uint32 NumberOfChild = 0;  
    if (PropertyHandle->GetNumChildren(NumberOfChild) == FPropertyAccess::Success)
    {
	    for (uint32 Index = 0; Index < NumberOfChild; ++Index)  
       {
	       TSharedRef<IPropertyHandle> ChildPropertyHandle = PropertyHandle->GetChildHandle(Index).ToSharedRef();  
          ChildBuilder.AddProperty(ChildPropertyHandle);  
       }
    }
}
```

## Get context
You may want to know in what BP/Actor this property is being rendered
Here is how aquanox does for his [BlueprintComponentReference](https://github.com/aquanox/BlueprintComponentReferencePlugin/blob/main/Source/BlueprintComponentReferenceEditor/BlueprintComponentReferenceCustomization.cpp#L301) plugin


## Add new rows
See `ChildBuilder.AddCustomRow`