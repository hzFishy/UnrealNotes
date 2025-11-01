
- [Editor Validators](https://zomgmoz.tv/unreal/Testing/Editor-Validators)

# Validate Assets
You can use IsDataValid to generated BP compilation message/warning/error as you need (for example, to tell your designers they missed something).

## Fix actions
In 5.5 there is a new "fix me" overridable methods that will run code when you click on the text (similar to engine "fix" clickable text), [more info here](https://github.com/Flassari/CommonValidators/pull/3/files).

Example:
```c++
TSharedRef<FTokenizedMessage> Message = FTokenizedMessage::Create(EMessageSeverity::Error);  
Message->AddToken(FAssetNameToken::Create(GetPathNameSafe(Blueprint),  
    FText::FromString(FString::Printf(  
       TEXT("Prefab Reference Component %s doesn't have the default required struct FPSRequiredActorPrefabSpawnParameters (Required Spawn Prefab Parameters)."),  
       *FU_Utilities::GetNameWithoutTemplateSuffix(PRC->GetFName()).ToString()  
))));  
  
// Make a FixeController, this can containe multiple fixs
TSharedRef<FMutuallyExclusiveFixSet> FixController = MakeShareable(new FMutuallyExclusiveFixSet());

// fix function  
TFunction<FFixResult()> ApplyFix1 = [PRC]()
{
    ...
    return FFixResult::Success();
};
// fix function
TFunction<FFixResult()> ApplyFix2 = [PRC]()
{
    ...
    return FFixResult::Success();
};

// action message with fixer 1
FixController->Add(INVTEXT(
	"Click to apply fix1"),
	MakeFix(MoveTemp(ApplyFix1))
);

// action message with fixer 2
FixController->Add(INVTEXT(
	"Click to apply fix2"),
	MakeFix(MoveTemp(ApplyFix2))
);

FixController->CreateTokens([Message](TSharedRef<FFixToken> FixToken)  
{
	Message->AddToken(FixToken);  
});

// finally, add the message to the editor validator entries  
Context.AddMessage(Message);
```

Result:
![[Pasted image 20250708001418.png]]


# Validate project

You can use validators
See [CommonValidators](https://github.com/Flassari/CommonValidators)

## Map checks
[CheckForErrors](https://zomgmoz.tv/unreal/Testing/CheckForErrors)

# TValueOrError

[example code](https://x.com/OutoftheboxP/status/1703032001794585060)

