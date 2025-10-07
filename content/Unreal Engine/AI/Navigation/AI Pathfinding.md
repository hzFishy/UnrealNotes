See also [[AI navigation]].

# About
`AAIController::FindPathForMoveRequest` (called from `AAIController::MoveTo`) calls `UNavigationSystemV1::FindPathSync`.

## Merging
If `bStartFromPreviousPath` is true on the `FAIMoveRequest` the result of `FindPathForMoveRequest` in `MoveTo` will be merged with the current path of the `PathFollowingComponent`.
Then it will call `AAIController::RequestMove`.
`AAIController::RequestMove` calls `UPathFollowingComponent::RequestMove`.

# Types

## `FNavigationPath`

## `FPathFindingResult`