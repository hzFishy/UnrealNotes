
# Resources
- [Zomg's post](https://zomgmoz.tv/unreal/Editor-customization/Creating-custom-property-editors-for-structs-and-other-types)


## Get property
This works if the property has a `Edit` specifier
```c++
TSharedPtr<IPropertyHandle> SelectedSocketProp = PropertyHandle->GetChildHandle(GET_MEMBER_NAME_CHECKED(FFUPickerSockets, SelectedSocket));  
  
FName SelectedSocket;  
SelectedSocketProp->GetValue(SelectedSocket);
```