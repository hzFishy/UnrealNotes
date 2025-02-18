

# In-depth
## Pathfinding
- `AAIController::FindPathForMoveRequest` calls `UNavigationSystemV1::FindPathSync`


# Miscs
## AI not playing animations
enable `bUseAccelerationForPaths` on the CMC

## AI braking
to change the AI braking you have on the Nav Component (can be CMC)
- `bUseFixedBrakingDistanceForPaths`
- `FixedPathBrakingDistance`


# `MoveTo` params

## `UAITask_MoveTo` params
- `bAllowPartialPath`: "allow using incomplete path going toward goal but not reaching it"
- `bool bLockAILogic`: ~"stops BT's (and probably ST's) from running while the move is active"
- `bUseContinuousGoalTracking`: if a goal actor is set and valid the AI will continuously try to go towards it. Can fail or be externally cancel.
- `ProjectGoalOnNavigation`: "Try to move the goal to the navigation surface before requesting the move, fails if it can't." / "goal location will be projected on navigation data before use"
- `RequireNavigableEndLocation`: "Set to **No/Disable** to allow pursuing the request even if no navigation surface is found at the goal location." / "if set - require the end location to be linked to the navigation data"

## Other info
- `bCanStrafe`: "keep focal point at move goal"

