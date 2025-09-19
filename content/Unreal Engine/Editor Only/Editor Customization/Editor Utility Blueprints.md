
 
# Resources
- [Getting Started with Editor Utility Blueprints](https://dev.epicgames.com/community/learning/tutorials/owYv/unreal-engine-getting-started-with-editor-utility-blueprints)
- [Scripting the Unreal Editor using Blueprints](https://dev.epicgames.com/documentation/en-us/unreal-engine/scripting-the-unreal-editor-using-blueprints)

# About
Using blueprint utilities its super simple to create common workflow actions.

> [!Danger] Transactions
> It is important to use the `Begin Transaction`, `Transact Object` and `End Transaction` nodes when modifying an object (such as an asset) to allow the user to undo their actions.

# Examples

## Content Browser Actions
### Bulk edit selected static meshes with given material(s)

**Final result**
<br>![[Data/BulkSetMaterials.gif]]<br>

**How to make it**
- Create a new `Editor Utility Blueprint` in the content browser (In `Editor Utilities`)
- Pick the `Asset Action Utility` parent class
- Open the blueprint and in the `Class Defaults` add a new entry in the `Supported Classes` array and set it to the `Static Mesh` type.
- Add a new function, name it correctly as it will be displayed in the asset actions *(you can also write a description in the function details, it will be showed on hover)*
- Copy [this](https://blueprintue.com/blueprint/jbg3dslj/) code inside the function (here named `BulkSetMaterial`)
- The function input is named `Materials` and is of type `Material Interface (Array)` *(We use `Material Interface` type so we can accept any type of material, such as `Material Instance`)*.


> [Video for more details](https://www.youtube.com/watch?v=qkmek7kPup4)
