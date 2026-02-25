
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
