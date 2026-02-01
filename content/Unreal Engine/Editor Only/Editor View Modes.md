
View modes (editor or game) calls `ApplyViewMode` afterwards.
See also `EViewModeIndex`.

# Lighting Only
- `LightingOnlyOverride` flag
- `VMI_LightingOnly`

If true `FEditorViewportClient::SetupViewForRendering` will set `DiffuseOverrideParameter` and `SpecularOverrideParameter` on the `FSceneView`.
It will use `UEngine` `LightingOnlyBrightness` variable. Which is set using the `LightingOnlyBrightness` value in the `ini` file (default is R=0.3,G=0.3,B=0.3,A=1.0).

Those "override" variables are used deep down in render, found traces in `SetupCommonViewUniformBufferParameters` and `VIEW_UNIFORM_BUFFER_MEMBER_EX` in `VIEW_UNIFORM_BUFFER_MEMBER_TABLE`.

This is used in `FViewUniformShaderParameters`.

