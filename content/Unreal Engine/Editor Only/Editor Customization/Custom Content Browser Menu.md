
# Resources
- See `UGameplayEffectCreationMenu` for an example.


# Simple Injecting

![[Pasted image 20260225202806.png]]

```c++
FContentBrowserModule& ContentBrowserModule = FModuleManager::LoadModuleChecked<FContentBrowserModule>("ContentBrowser");
	ContentBrowserModule.GetAllAssetContextMenuExtenders().Add(
		FContentBrowserMenuExtender_SelectedPaths::CreateLambda([] (const TArray<FString>& SelectedPaths)
		{
			TSharedRef<FExtender> Extender = MakeShared<FExtender>();
			
			// Add generic menu
			Extender->AddMenuExtension(
				"ContentBrowserNewAdvancedAsset",
				EExtensionHook::Before,
				TSharedPtr<FUICommandList>(),
				FMenuExtensionDelegate::CreateLambda([] (FMenuBuilder& ExtenderBuilder)
				{
					ExtenderBuilder.AddMenuSeparator();
					ExtenderBuilder.AddSubMenu(
						INVTEXT("AARootMenu"),
						INVTEXT("AA"),
						FNewMenuDelegate::CreateLambda([] (FMenuBuilder& RootMenuBuilder)
						{
							RootMenuBuilder.AddMenuEntry(
								INVTEXT("Test1"),
								INVTEXT("Test 1"),
								FSlateIcon(),
								FUIAction(FExecuteAction::CreateLambda([](){}))
							);
							
							RootMenuBuilder.AddMenuEntry(
								INVTEXT("Test2"),
								INVTEXT("Test 2"),
								FSlateIcon(),
								FUIAction(FExecuteAction::CreateLambda([](){}))
							);
							
						})
					);
				})
			);
			return Extender;
		}));
```

# Creating an asset

Basically you need a package name (Folder destination), an asset name (file name) and a factory (used to create the asset).

Here is an example to create a Blueprint Asset and a Data Asset.

```c++

// Here the BasePath (or Path) used comes from the SelectedPaths param that FContentBrowserMenuExtender_SelectedPaths returns.
// The first entry should always be valid so its safe to cache it early as FString Path = SelectedPaths[0].

void FContentBrowserExtender::CreateGenericAsset(FString BasePath, FString AssetName, UClass* CoreClass, UFactory* Factory)
{
	FString PackageName = BasePath;
	FString DefaultFullPath = PackageName / AssetName;
	
	FAssetToolsModule& AssetToolsModule = FAssetToolsModule::GetModule();
	
	AssetToolsModule.Get().CreateUniqueAssetName(
		DefaultFullPath,
		TEXT(""),
		PackageName,
		AssetName
	);
	
	auto& LocalContentBrowserModule = FModuleManager::LoadModuleChecked<FContentBrowserModule>("ContentBrowser");
	LocalContentBrowserModule.Get().CreateNewAsset(
		*AssetName,
		BasePath,
		CoreClass,
		Factory
	);
}

void FContentBrowserExtender::CreateBlueprint(FString BasePath, UClass* Class, FString AssetName)
{
	auto* BlueprintFactory = NewObject<UBlueprintFactory>();
	BlueprintFactory->ParentClass = Class;
	CreateGenericAsset(BasePath, AssetName, UBlueprint::StaticClass(), BlueprintFactory);
}

void FContentBrowserExtender::CreateDataAsset(FString BasePath, UClass* Class, FString AssetName)
{
	auto* DataAssetFactory = NewObject<UDataAssetFactory>();
	DataAssetFactory->DataAssetClass = Class;
	CreateGenericAsset(BasePath, AssetName, UDataAsset::StaticClass(), DataAssetFactory);
}

// Usage
CreateBlueprint(Path, UCharacterGameplayAbility::StaticClass(), "GA_");
CreateBlueprint(Path, UGameplayEffect::StaticClass(), "GE_");
CreateDataAsset(Path, UCharacterSkillDefinition::StaticClass(), "DA_");
```
