thanks
# Using external data
You cannot really store any data inside a Gameplay Ability once its granted to get it back when its executed because depending on your execution policy a new instance will be created.

> See [Tranek Docs: Passing Data to Abilities](https://open.spotify.com/track/4XKYLo1eAUFETIt5PLy8ZG?si=c4b38706dcc84fa7)
> To actvate a GA by event see [this section](https://github.com/tranek/GASDocumentation?tab=readme-ov-file#464-activating-abilities)

# Activation
Here we will follow the path of `UAbilitySystemComponent::TryActivateAbility` to see when we check for the requirements.
`UAbilitySystemComponent::TryActivateAbility` calls `UAbilitySystemComponent::InternalTryActivateAbility` which calls `UGameplayAbility::CanActivateAbility`. This function will run a lof of checks including `UGameplayAbility::DoesAbilitySatisfyTagRequirements` which calls the `CheckForRequired` and `CheckForBlocked` lambdas.
