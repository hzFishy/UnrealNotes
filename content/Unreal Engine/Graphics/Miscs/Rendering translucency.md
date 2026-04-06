
You can easily have translucency ordering issues.
For example you might have ISMs that are quite large, and depending on your camera position and view, one mesh will be rendered on top of the other, while it is "behind" the other ISM(s).

You can tweak `TranslucencySortPriority` and `TranslucencySortDistanceOffset` on primitive components to fix this issue, if possible in your project.
