

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

