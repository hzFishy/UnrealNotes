
# Resources
- [How to lazy load the ASC](https://vorixo.github.io/devtricks/lazy-loading-asc/)

# Delegates
> See details about each delegate in source code
- Related to GEs
	- `OnGameplayEffectAppliedDelegateToSelf`
	- `OnGameplayEffectAppliedDelegateToTarget`
	- `OnActiveGameplayEffectAddedDelegateToSelf`
	- `OnPeriodicGameplayEffectExecuteDelegateOnSelf`
	- `OnPeriodicGameplayEffectExecuteDelegateOnTarget`
	- `OnImmunityBlockGameplayEffectDelegate`
	- `OnAnyGameplayEffectRemovedDelegate`
	- `OnGameplayEffectRemoved_InfoDelegate`
	- `OnGameplayEffectStackChangeDelegate`
	- `OnGameplayEffectTimeChangeDelegate`
	- `OnGameplayEffectInhibitionChangedDelegate`
- Related to Attributes
	- `GetGameplayAttributeValueChangeDelegate`
- Related to GAs
	- `AbilityActivatedCallbacks`
	- `AbilityEndedCallbacks`
	- `OnAbilityEnded` (has more info than `AbilityEndedCallbacks`)
	- `AbilityCommittedCallbacks`
	- `AbilityFailedCallbacks`
	- `AbilitySpecDirtiedCallbacks`
	- `GameplayEffectApplicationQueries`

# Granting Abilities
You can grant an ability before the ASC was initialized with an avatar and owning actor.
Abilities are stored in `ActivatableAbilities`.
