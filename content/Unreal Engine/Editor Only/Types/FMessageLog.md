# Register new log

```c++

// On module startup
FMessageLogModule& MessageLogModule = FModuleManager::LoadModuleChecked<FMessageLogModule>("MessageLog");
MessageLogModule.RegisterLogListing("MyLogName", INVTEXT("My Log Name"));

// On module shutdown
MessageLogModule.UnregisterLogListing("MyLogName");

```

# Usage
Simple do `FMessageLog("MyLogName")` and use the various functions available.

`NewPage` allows to create a new page (Page button at the bottom middle).


Examples

```c++
// basic text
FMessageLog("FishyUtils").Info()  
	->AddText(INVTEXT("All info about Primitive Components with bGenerateOverlapEvents enabled for all project assets"));


// tokens
FMessageLog("FishyUtils").Info()
	->AddText(FText::FromString(FString::Printf(TEXT("[%s] %s"), *GenerateOverlapEventsResultToString(Result), *FU::Utils::GetObjectDetailedName(Component))))
	->AddToken(FActionToken::Create(INVTEXT("Open Blueprint"), INVTEXT("Open Blueprint"), FOnActionTokenExecuted::CreateLambda([Asset] ()
		{
			GEditor->GetEditorSubsystem<UAssetEditorSubsystem>()->OpenEditorForAsset(Asset.GetAsset());
		})))
	->AddToken(FUObjectToken::Create(Asset.GetAsset()))
;
```

