
You can create a new handle with the property module.

```c++
auto& PropertyModule = FModuleManager::LoadModuleChecked<FPropertyEditorModule>("PropertyEditor");

auto SingleProperty = PropertyModule.CreateSingleProperty(
	// The object which has the property
	Metadata,
	// The name of the property you want to get from Metadata
	GET_MEMBER_NAME_CHECKED(UCAIBPatrolSplineMetadata, SplinePointsData),
	// Extra params
	FSinglePropertyParams()
);

TSharedPtr<IPropertyHandle> NewHandle = SingleProperty->GetPropertyHandle();
```
