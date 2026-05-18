
# Create Shapes

Shapes commands are registered via `REGISTER_MODELING_TOOL_COMMAND` as `FUICommandInfo`.

When you click to create a shape, `UAddPrimitiveToolBuilder::BuildTool` is called.
This will create a new subclass object of `UAddPrimitiveTool` (for example `UAddTorusPrimitiveTool` is used for a Torus shape).

The manager will then call `UInteractiveTool::Setup`.

`UAddPrimitiveTool::GenerateMesh` is used to generate the actual mesh the tool is building.

