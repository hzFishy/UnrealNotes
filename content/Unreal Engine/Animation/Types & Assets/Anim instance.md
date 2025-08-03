

# Design philosophy
- See [Vaei - Animation templates](https://github.com/Vaei/LocoTips/wiki/Building-Your-System#animation-templates)
- See [Vaei - Animation States design](https://github.com/Vaei/LocoTips/wiki/Animation-States#implementation)
- instead of switching anim layer to match equipped weapon (aka 1 anim layer per weapon), you stay in the same graph BUT you update a given container (for example a data asset)
- gather your data in `UAnimInstance::NativeUpdateAnimation` (called on game thread)
- use your collected data in `UAnimInstance::NativeThreadSafeUpdateAnimation` (called on worker threads)

# Montage behavior
*Thanks to zomg on the Unreal Source Discord for some insights after hitting this problem*

You can know if any montages are running (using a class reference) by using `GetInstanceForMontage` or `GetActiveInstanceForMontage` ("instance" being the actual montage running).

But watch out, when the running instance is blending out, the anim instance will not consider it as active.

So if for example you want to check if a montage is currently running (active or blending out), and if so run another montage:
```c++
UAnimInstance* AnimInstance = GetMesh()->GetAnimInstance();
if (auto* CurrentAnim = AnimInstance->GetInstanceForMontage(NPCMontages->PickupMontage))
{
	FOnMontageEnded PlayEndDelegate = FOnMontageEnded::CreateWeakLambda(this, [PlayPickupAnim](UAnimMontage*, bool)
	{
		PlayPickupAnim();
	});
	CurrentAnim->OnMontageEnded = PlayEndDelegate;
}
else
{
	PlayPickupAnim();
}
```

