
# Resources
- [First-Person Rendering in Unreal Engine 5.6 | Unreal Fest Orlando 2025](https://www.youtube.com/watch?v=11sLIyw0pWQ)
- [[Animation offset for first person]]


# Usage
There are new parameters on the camera component and on the primitive components.

## Primitive Component
For the primitives, you now have a variable called `FirstPersonPrimitiveType` which by default is set to `None`.

> [!Info] `FirstPersonPrimitiveType` description
> "If this is set to FirstPerson, the camera FirstPersonFieldOfView and FirstPersonScale parameters will be used on this component. These parameters can be used to render the component with a different field of view and a smaller depth range such that clipping with the scene can be avoided. This is useful for rendering first person view geometry."

Available values:
- `None`: "Primitive does not interact with first person rendering."
- `FirstPerson`: "Primitive is rendered as first person and affected by first person properties on the camera."
- `WorldSpaceRepresentation`: "Primitive represents a first person primitive in world space. This is the primitive that other players would see and is used to cast shadow onto the ground among other things. Implicitly sets bCastHiddenShadow=false and bOwnerNoSee=true internally, which is required for first person shadow to work correctly with VSM."

## Camera
The camera has a few new parameters

- `bEnableFirstPersonFieldOfView`: "True if the first person field of view should be used for primitives tagged as `IsFirstPerson`."
- `bEnableFirstPersonScale`: "True if the first person scale should be used for primitives tagged as `IsFirstPerson`."
- `FirstPersonFieldOfView`: "The horizontal field of view (in degrees) used for primitives tagged as `IsFirstPerson`."
- `FirstPersonScale`: "The scale to apply to primitives tagged as `IsFirstPerson`. This is used to scale down primitives towards the camera such that they are small enough not to intersect with the scene."
