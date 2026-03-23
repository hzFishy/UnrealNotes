
# Duration
Duration stuff is mostly done in `FActiveGameplayEffectsContainer::ApplyGameplayEffectSpec`.
The container will register a delegate to `UAbilitySystemComponent::CheckDurationExpired` for the calculated duration (this only calls `FActiveGameplayEffectsContainer::CheckDuration` in 5.6).

# Execution
See `FActiveGameplayEffectsContainer::ExecuteActiveEffectsFrom`.

## Periodic execution
See `UAbilitySystemComponent::ExecutePeriodicEffect` and `FActiveGameplayEffectsContainer::ExecutePeriodicGameplayEffect`.

# Stacking
When calling `UAbilitySystemComponent::ApplyGameplayEffectSpecToTarget` or `UAbilitySystemComponent::ApplyGameplayEffectSpecToSelf` it will eventually call `FActiveGameplayEffectsContainer::ApplyGameplayEffectSpec` which will try to find a stackable effect with `FActiveGameplayEffectsContainer::FindStackableActiveGameplayEffect`.

This means that the returned `FActiveGameplayEffectHandle` will be pointing to the already applied GE if it was stacked, otherwise its a "new" handle pointing to a "new" active GE.

> [!Warning] Warning
> The new GE Spec you passed to `ApplyGameplayEffectSpecToSelf` will override the previous GE spec of the already active GE.

# Miscs
See `UGameplayEffectCreationMenu` to add custom menu entries in the Content Browser for your gameplay effects.

