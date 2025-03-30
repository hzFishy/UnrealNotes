
# Component Visualizer
There is a struct called `FComponentVisualizer` that seems to be used to draw/debug whatever you want for a specific component class.

See `FConstraintComponentVisualizer::DrawVisualization` and `FComponentVisualizersModule::StartupModule` for an example.

See also [zomg's post](https://zomgmoz.tv/unreal/Editor-customization/Component-visualizers) on it.

> [!Warning] About `GUnrealEd` in `StartupModule`
> As I found [here](https://forums.unrealengine.com/t/gunrealed-is-null-in-startupmodule/295355/2?u=hzfishy ) `GUnrealEd` is null in `StartupModule` if you are loading it to early.
> Load your module at `PostEngineInit` phase to correct that.

# Actor selection
[Forum thread on it](https://forums.unrealengine.com/t/can-an-aactor-react-on-ineditor-select/143706/3)
Notes:
- There is `Actor::IsSelectedInEditor
- There is `USelection::SelectionChangedEvent` and `USelection::SelectObjectEvent`
