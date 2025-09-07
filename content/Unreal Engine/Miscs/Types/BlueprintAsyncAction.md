
# Classes

## `UBlueprintAsyncActionBase`
Base class for async actions.

> BlueprintCallable factory functions for classes which inherit from UBlueprintAsyncActionBase will have a special blueprint node created for it: UK2Node_AsyncAction.
> You can stop this node spawning and create a more specific one by adding the UCLAS metadata "HasDedicatedAsyncNode"

## `UCancellableAsyncAction`
Use this if you async action can be aborted.

> Base class for asynchronous actions that can be spawned from UK2Node_AsyncAction or C++ code.
> These actions register themselves with the game instance and need to be explicitly canceled or ended normally to go away.
> The ExposedAsyncProxy metadata specifies the blueprint node will return this object for later canceling.


# Examples

Snippet:
```cpp
// header
DECLARE_DYNAMIC_MULTICAST_DELEGATE_OneParam(FTFCOnLobbyCreatedOutputPin, bool, Successful);


/**
 * 
 */
UCLASS()
class TOOTHFORCASH_API UTFCAsyncCreateLobby : public UBlueprintAsyncActionBase
{
	GENERATED_BODY()

public:
	UFUNCTION(BlueprintCallable, DisplayName="TFC Async Create Lobby", meta = (WorldContext = "WorldContext", BlueprintInternalUseOnly = "true"), Category="Tooth For Cash|Lobby")
	static UTFCAsyncCreateLobby* AsyncCreateLobby(UObject* WorldContext);

	UPROPERTY(BlueprintAssignable, DisplayName = "On Lobby Created")
	FTFCOnLobbyCreatedOutputPin OnLobbyCreatedDelegate;
	
private:
	virtual void Activate() override;

	UFUNCTION()
	void OnLobbyCreatedCompletedCallback(bool bWasSuccessful, const FString& Message);

	UPROPERTY()
	UObject* WorldContext = nullptr;

	UPROPERTY()
	TObjectPtr<UTFCOnlineSubsystem> OnlineSubsystem = nullptr;

	FDelegateHandle DelegateHandle;
};

// cpp
UTFCAsyncCreateLobby* UTFCAsyncCreateLobby::AsyncCreateLobby(UObject* WorldContext)
{
	UTFCAsyncCreateLobby* Node = NewObject<UTFCAsyncCreateLobby>();
	Node->WorldContext = WorldContext;
	
	return Node;
}

void UTFCAsyncCreateLobby::Activate()
{
	Super::Activate();

	if (WorldContext)
	{
		OnlineSubsystem = WorldContext->GetWorld()->GetGameInstance()->GetSubsystem<UTFCOnlineSubsystem>();
		DelegateHandle = OnlineSubsystem->OnLobbyCreatedDelegate.AddUObject(this, &ThisClass::OnLobbyCreatedCompletedCallback);
		OnlineSubsystem->CreateLobby(4);
	}
}

void UTFCAsyncCreateLobby::OnLobbyCreatedCompletedCallback(bool bWasSuccessful, const FString& Message)
{
	OnLobbyCreatedDelegate.Broadcast(bWasSuccessful);
	OnlineSubsystem->OnLobbyCreatedDelegate.Remove(DelegateHandle);
}
```
