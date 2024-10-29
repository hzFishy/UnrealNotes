
Code snippets (thanks to Snaps)

**Some parent class**
```cpp
void UFGItemContainer::GetReplicatedCustomConditionState(FCustomPropertyConditionState& OutActiveState) const
{
    DOREPDYNAMICCONDITION_INITCONDITION_FAST(ThisClass, ItemStacks, GetReplicationCondition());
    if(GetReplicationCondition() == COND_Custom)
    {
        DOREPCUSTOMCONDITION_ACTIVE_FAST(ThisClass, ItemStacks, ShouldReplicate());
    }
}

void UFGItemContainer::GetLifetimeReplicatedProps(TArray<FLifetimeProperty>& OutLifetimeProps) const
{
    FDoRepLifetimeParams Params;
    Params.bIsPushBased = true;
    Params.Condition = COND_Dynamic;
    
    DOREPLIFETIME_WITH_PARAMS_FAST(UFGItemContainer, ItemStacks, Params);
}

ELifetimeCondition UFGItemContainer::GetReplicationCondition() const
{
    return ELifetimeCondition::COND_Custom;
}

bool UFGItemContainer::ShouldReplicate() const
{
    return ViewingPlayers.Num() > 0;
}
```


**Some subclass where we want to override**
```cpp
ELifetimeCondition UFGItemBackpack::GetReplicationCondition() const
{
    return ELifetimeCondition::COND_OwnerOnly;
}

bool UFGItemBackpack::ShouldReplicate() const
{
    return true;
}
```