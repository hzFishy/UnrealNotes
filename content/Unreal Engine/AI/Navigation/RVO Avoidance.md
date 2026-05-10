
# Types

## `UAvoidanceManager`
Created in `UWorld::InitWorld`, use `UWorld::GetAvoidanceManager` to get it.
The class can be changed in the `Engine.ini` file.

```ini
# Default
[/Script/Engine.Engine]
AvoidanceManagerClassName=/Script/Engine.AvoidanceManager
```

### More

> [!Info]- More about AvoidanceWeight
> As described at `UAvoidanceManager::RegisterMovementComponent`: "When avoiding each other, actors divert course in proportion to their relative weights. Range is 0.0 to 1.0. Special: at 1.0, actor will not divert course at all"

## `IRVOAvoidanceInterface`
 Interface for objects that want to perform RVO avoidance. See `UCharacterMovementComponent` and `UWheeledVehicleMovementComponent` for example implementations.

## Miscs

### `FNavAvoidanceData`
There seems to be one instance of this struct per managed `IRVOAvoidanceInterface` (See `UAvoidanceManager::RegisterMovementComponent` and `UAvoidanceManager::UpdateRVO`).
Stored in `AvoidanceObjects` on the `UAvoidanceManager`.

Most of its variables are initialized from the getters of the `IRVOAvoidanceInterface` interface.

### `FNavAvoidanceMask`

### `FVelocityAvoidanceCone`


# Usages

## Character Movement Component
To see an example of implementation in the engine lest look the `UCharacterMovementComponent`.
It has a `bUseRVOAvoidance` variable (Note: RVO Avoidance only runs on the server).

Inside `UCharacterMovementComponent::SetUpdatedComponent` if `bUseRVOAvoidance` is true we get the avoidance manager and call `UAvoidanceManager::RegisterMovementComponent`.

Inside `UCharacterMovementComponent::TickComponent` if `bUseRVOAvoidance` is true `UCharacterMovementComponent::UpdateDefaultAvoidance` is called.
It will call `UAvoidanceManager::UpdateRVO` if `bWasAvoidanceUpdated` is false as well as `UCharacterMovementComponent::SetAvoidanceVelocityLock`.


Inside `UCharacterMovementComponent::CalcVelocity` if `bUseRVOAvoidance` is true `UCharacterMovementComponent::CalcAvoidanceVelocity` is called.
Unless we are locked (see `AvoidanceLockTimer`) we get our new velocity with `UAvoidanceManager::GetAvoidanceVelocityForComponent`.
Then if `bUseRVOPostProcess` is true we call `UCharacterMovementComponent::PostProcessAvoidanceVelocity` which is only meant to be used by subclasses.
We then call `UCharacterMovementComponent::SetAvoidanceVelocityLock`, this is how the CMC handles overwriting whatever velocity was calculated, the important part is to look the duration of the lock.
It will call `UAvoidanceManager::UpdateRVO`.
This also sets `bWasAvoidanceUpdated` to true.

# How it works
From the usage of the `UCharacterMovementComponent` the main functions seem to to register the component, call update each frame and get the velocity when needed.

`UAvoidanceManager::GetAvoidanceVelocityForComponent` calls `UAvoidanceManager::GetAvoidanceVelocity_Internal` which does the path for RVO avoidance. The ignored UID is the id of the object that we want to update.

`UAvoidanceManager::UpdateRVO` calls `UAvoidanceManager::UpdateRVO_Internal` which updates the internal `FNavAvoidanceData` data.

# Debug and Commands

Console Commands:
- `AvoidanceDisplayAll` (Calls `UAvoidanceManager::HandleToggleDebugAll`)
- `AvoidanceSystemToggle` (Calls `UAvoidanceManager::HandleToggleAvoidance`)

For more precise (but manual) debugging see `UAvoidanceManager::AvoidanceDebugForUID`.
