
You need to use `IDetailCustomization`.

To do this, create a subclass of `IDetailCustomization`

Example:
```c++
class FPMGlobalClassDetailCustomization : public IDetailCustomization
{
	
public:
	static TSharedRef<IDetailCustomization> MakeInstance();

	virtual void CustomizeDetails(IDetailLayoutBuilder& DetailBuilder) override;
};
```

Then in your editor module register it:
```c++
FPropertyEditorModule& PropertyModule = FModuleManager::LoadModuleChecked<FPropertyEditorModule>("PropertyEditor");
	PropertyModule.RegisterCustomClassLayout(UPaperTileMapComponent::StaticClass()->GetFName(), FOnGetDetailCustomizationInstance::CreateStatic(&FPMGlobalClassDetailCustomization::MakeInstance));
```

And don't forget to use `UnregisterCustomClassLayout` on module shutdown.

