
# Duration
Duration stuff is mostly done in `FActiveGameplayEffectsContainer::ApplyGameplayEffectSpec`.
The container will register a delegate to `UAbilitySystemComponent::CheckDurationExpired` for the calculated duration (this only calls `FActiveGameplayEffectsContainer::CheckDuration` in 5.6).

# Execution
See `FActiveGameplayEffectsContainer::ExecuteActiveEffectsFrom`.

## Periodic execution
See `UAbilitySystemComponent::ExecutePeriodicEffect` and `FActiveGameplayEffectsContainer::ExecutePeriodicGameplayEffect`.

# Miscs
See `UGameplayEffectCreationMenu` to add custom menu entries in the Content Browser for your gameplay effects.

