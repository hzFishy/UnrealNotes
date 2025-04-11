For a editor VP solution see [[Custom show flag]]
# Component Visualizer
There is a struct called `FComponentVisualizer` that seems to be used to draw/debug whatever you want for a specific component class.

See `FConstraintComponentVisualizer::DrawVisualization` and `FComponentVisualizersModule::StartupModule` for an example.

See [zomg's post](https://zomgmoz.tv/unreal/Editor-customization/Component-visualizers) for the base setup.

> [!Warning] About `GUnrealEd` in `StartupModule`
> As I found [here](https://forums.unrealengine.com/t/gunrealed-is-null-in-startupmodule/295355/2?u=hzfishy ) `GUnrealEd` is null in `StartupModule` if you are loading it to early.
> Load your module at `PostEngineInit` phase to correct that.

More details about it (with an example on Unity box collider 6 faces maker) [here](https://dev.epicgames.com/community/learning/tutorials/KP5p/unreal-engine-extending-unreal-editor-with-component-visualizer)

