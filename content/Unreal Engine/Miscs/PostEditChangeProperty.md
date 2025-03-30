
Code snippet on how to use `PostEditChangeProperty` to know when a specific field is edited
```c++
#if WITH_EDITOR  
void UBPGTargetPortReferenceComponent::PostEditChangeProperty(struct FPropertyChangedEvent& PropertyChangedEvent)  
{  
    Super::PostEditChangeProperty(PropertyChangedEvent);
    
    FName PropertyName = (PropertyChangedEvent.Property != nullptr) ? PropertyChangedEvent.Property->GetFName() : NAME_None;  
  
    if (PropertyName == GET_MEMBER_NAME_CHECKED(ThisClass, TargetPortActorClass))  
    {
	    // ...
    }  
}
#endif
```
