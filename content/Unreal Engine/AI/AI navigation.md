

# In-depth
## Pathfinding
- `AAIController::FindPathForMoveRequest` calls `UNavigationSystemV1::FindPathSync`

## `Corridor` and `String` (NavigationPath/Mesh)
*Thanks Bruno on the Unreal Source Discord*

A corridor is the same concept as in a flat corridor. Imagine a flat where the entrance door is on one side and you have different rooms alongside the flat. They are all connected by a corridor. If you path from the entrance to the last room, your path will start at the door, follow up the corridor until the end, then into the last room. 
Same concept in nav, except that the corridor walls are defined by NavEdges. There's also the concept of PathCorridorEdges, which are lines that connect NavEdges sides on both sides of the corridor. I marked the PathCorridorEdges in green, the NavEdges in red. They all conform data for the corridor. 
The corridor will only contain nav polygons/nav edges needed for your path and nothing else.
![[Pasted image 20250221110829.png]]

String refer to an actual string made of fabric, like a shoe lace.
When the detour library initially calculates a path, it will consider every edge towards the goal, like the red path in the image.
Imagine now that the red line is a string.
If you pull from it, it will tense, giving you a shorter path skipping unnecessary edge points, resulting in the green line.
![[Pasted image 20250221110948.png]]
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

