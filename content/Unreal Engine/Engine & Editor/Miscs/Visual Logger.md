
# Resources
- [Using the Visual Logger](https://unreal-garden.com/tutorials/visual-logger/)



# Notes

## Snapshots
See [this section](https://unreal-garden.com/tutorials/visual-logger/#actor-snapshot) and `IVisualLoggerDebugSnapshotInterface`.


# Events

## `FVisualLoggerFilters`

- `OnFilterCategoryAdded`
- `OnFilterCategoryRemoved`

# Editor Code Analysis

`SVisualLogger` is the core main slate widget.

`SVisualLoggerFilters` is the slate widget that holds the list of categories.
Inside `SVisualLoggerFilters::Construct` `FVisualLoggerFilters::Get().Categories` is iterated to add all the categories as `SVisualLoggerFilterWidget`.

`FVisualLoggerFilters::Initialize` is called in `FLogVisualizerModule::StartupModule`.
`FVisualLoggerFilters::AddCategory` and `FVisualLoggerFilters::RemoveCategory` is used to add/remove entries in the `Categories` array.

New categories are added by `FVisualLoggerFilters::OnNewItemHandler`, which is a callback set in `FVisualLoggerFilters::Initialize` on `FVisualLoggerDBEvents::OnNewItem` (stored in `FVisualLoggerDatabase`).

From what i gathered the flow is `FVisualLoggerDatabase::AddItem` -> `FVisualLoggerDBRow::AddItem` -> `OnNewItem is broadcasted` -> `FVisualLoggerFilters::OnNewItemHandler callback` -> `FVisualLoggerFilters::AddCategory`.

