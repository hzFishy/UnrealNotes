
## Gameplay Ability Activate Fail Tags

> See [section](https://github.com/tranek/GASDocumentation?tab=readme-ov-file#4642-activation-failed-tags) in tranek readme.
> See `UAbilitySystemGlobals` and `UGameplayAbilitiesDeveloperSettings`

- The `IsDead` fail version is deprecated since 5.5.
- The `XXXName` versions are deprecated, use the var with `XXXTag`.
- `ActivateFailCanActivateAbilityTag` was also added (in `UGameplayAbilitiesDeveloperSettings` but falls in the `AbilitySystemGlobals` ini section)

Here are is the updated tags to declare and to set in the settings:
```ini
# in your gameplay tags .ini file
# Note: The tags can be renamed
+GameplayTagList=(Tag="Activation.Fail.CanActivateFailed",DevComment="TryActive failed due to GameplayAbility's CanActivateAbility function (Blueprint or Native)")
+GameplayTagList=(Tag="Activation.Fail.OnCooldown",DevComment="TryActivate failed due to being on cooldown")
+GameplayTagList=(Tag="Activation.Fail.CantAffordCost",DevComment="TryActivate failed due to not being able to spend costs")
+GameplayTagList=(Tag="Activation.Fail.Networking",DevComment="Failed to activate due to invalid networking settings, this is designer error")
+GameplayTagList=(Tag="Activation.Fail.BlockedByTags",DevComment="TryActivate failed due to being blocked by other abilities")
+GameplayTagList=(Tag="Activation.Fail.MissingTags",DevComment="TryActivate failed due to missing required tags")

# in your DefaultGame.ini file
# values can either be XXX=(TagName="Your.Tag") or XXX=Your.Tag
[/Script/GameplayAbilities.AbilitySystemGlobals]
ActivateFailCanActivateAbilityTag=(TagName="Activation.Fail.CanActivateFailed")
ActivateFailCooldownTag=(TagName="Activation.Fail.OnCooldown")
ActivateFailCostTag=(TagName="Activation.Fail.CantAffordCost")
ActivateFailNetworkingTag=(TagName="Activation.Fail.Networking")
ActivateFailTagsBlockedTag=(TagName="Activation.Fail.BlockedByTags")
ActivateFailTagsMissingTag=(TagName="Activation.Fail.MissingTags")
```

> [!Info] You can see those tags being used on the second ability system category

