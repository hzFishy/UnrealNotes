
# Component Visualizer
There is a struct called `FComponentVisualizer` that seems to be used to draw/debug whatever you want for a specific component class.

See `FConstraintComponentVisualizer::DrawVisualization` and `FComponentVisualizersModule::StartupModule` for an example.

See also [zomg's post](https://zomgmoz.tv/unreal/Editor-customization/Component-visualizers) on it.

# Actor selection

[Forum thread on it](https://forums.unrealengine.com/t/can-an-aactor-react-on-ineditor-select/143706/3)
Notes:
- There is `Actor::IsSelectedInEditor
- There is `USelection::SelectionChangedEvent` and `USelection::SelectObjectEvent`
