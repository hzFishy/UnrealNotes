
> [!Warning] Anim notifies are not guaranteed to be fired
> Good explanation by *Vaei*:
> 
> *"Don't use notifies for important events.
> A lot of the time they simply don't fire, you'll start seeing then when testing game in actual production. 
> Setting to branching point notify ensures they always fire.
> However branching point notifies don't fire if there are 2 on the same frame, especialy common when its the start of a montage.
> Use my task node/plugin instead, it basically looks for notifies of a specific type, caches the time where they should play, and starts a timer, and broadcasts an event"*
> 
> See [PlayMontageAdvanced](https://github.com/Vaei/PlayMontageAdvanced/)


# Create your own Notify

First, make a subclass of `UAnimNotify` and add your own delegate and call it in the `Notify` override.
```c++
class PUSHTHEMALL_API UPTAEquipmentItemUseNotify : public UAnimNotify  
{  
    GENERATED_BODY()  
  
public:  
    virtual void Notify(USkeletalMeshComponent* MeshComp, UAnimSequenceBase* Animation, const FAnimNotifyEventReference& EventReference) override;  
};
```

Then you can know if an anim montage is using it by casting the notify events:
```c++
const auto& NotifyEvents = UseMontage->Notifies;  
for (auto& NotifyEvent : NotifyEvents)  
{
	if (const auto& UseNotify = Cast<UPTAEquipmentItemUseNotify>(NotifyEvent.Notify))  
    {
	    // Some callback setup  
    }  
}
```
