
# Actor Factories
**Actor** Factory classes allows you do do custom behavior when drag and dropping your assets into the level.

Editor example : When you drag and drop a Static Mesh asset into a level, the editor is spawning a Static Mesh Actor and setting the static mesh to the asset your just dropped.

Another use case would be to use Data Assets.

Code snippets:
*(thanks to duroxxigar)*

```c++ title=".h"
UCLASS()
class PROJECTEDITOR_API UActorFactoryPickup : public UActorFactory
{
    GENERATED_BODY()
    
public:
    UActorFactoryPickup();
    virtual bool CanCreateActorFrom( const FAssetData& AssetData, FText& OutErrorMsg ) override;
    virtual FString GetDefaultActorLabel(UObject* Asset) const override;
    virtual void PostSpawnActor(UObject* Asset, AActor* NewActor) override;
};
```

```c++ title=".cpp"
UActorFactoryPickup::UActorFactoryPickup()
{
    NewActorClass = AStnWorldPickup::StaticClass();
}

bool UActorFactoryPickup::CanCreateActorFrom(const FAssetData& AssetData, FText& OutErrorMsg)
{
    return AssetData.IsInstanceOf(UStnItemData::StaticClass());
}

FString UActorFactoryPickup::GetDefaultActorLabel(UObject* Asset) const
{
    return FString(TEXT("WorldPickup"));
}

void UActorFactoryPickup::PostSpawnActor(UObject* Asset, AActor* NewActor)
{
    Super::PostSpawnActor(Asset, NewActor);

    if (auto* pickup = Cast<AStnWorldPickup>(NewActor))
    {
        if (auto* asset = Cast<UStnItemData>(Asset))
        {
            auto inventoryItem = FInventoryItem(asset);
            if (auto* gunAsset = Cast<UGunData>(Asset))
            {
                auto data = FGunRunningData();
                data.CurrentAmmo = gunAsset->MagazineSize;
                inventoryItem.RuntimeData = FInstancedStruct::Make<FGunRunningData>(data);
            }
            pickup->SetItemDefinition(inventoryItem);
        }
        else
        {
            UE_LOGFMT(LogActorFactory, Log, "Cannot spawn {0} as actor: {1}", Asset->GetFullName(), NewActor->GetFullName());
        }
    }
}
```

In his case, the factory isn't registered in the editor module (and not in any other module)

