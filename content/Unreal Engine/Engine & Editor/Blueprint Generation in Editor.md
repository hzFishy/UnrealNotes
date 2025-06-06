
> [!Info] About Slate classes
> Since all slate widgets are contained in `TSharedPtr`s, I won't always type `TSharedPtr<SSomeWIdget>` but `SSomeWidget`.

> [!Warning] Small disclaimer
> This topic has a LOT going on under the hood, I won't mention EVERYTHING that happens when doing X with Y, I talk about the important/relevant stuff happening.

# Base

## My Blueprint
When you open an BP in editor it runs `FBlueprintEditorModule::CreateBlueprintEditor` which creates a `TSharedRef<FBlueprintEditor>`.

The "My Blueprint" tab slate widget is made in `FMyBlueprintSummoner::CreateTabBody` which returns a `SMyBlueprint` (that was previously made by `FBlueprintEditor`)

## Categories and content
The categories "GRAPHS", "FUNCTIONS", "MACROS", "VARIABLES" and "EVENT DISPATCHERS" are all of type `SCategoryHeaderTableRow` and the entries inside are `STableRow<TSharedPtr<FGraphActionNode>>`
Example:
<br>![[Pasted image 20250606002634.png]]<br>
## "+" buttons
The "+" buttons of each of these categories are made in `SMyBlueprint::CreateAddToSectionButton` (its called on Paint)

When you click on any of them the call-back will be `SMyBlueprint::OnAddButtonClickedOnSection`. It then runs a `ExecuteAction` on the `CommandList` member var of type `FUICommandList` using the `FBlueprintEditorCommands`, the only thing that changes between the different categories "+" buttons is what `AddNewXXX` `FUICommandInfo` will be called (for example for the "+" variable button it gets `AddNewVariable`).

> [!Info] These commands are made in `FBlueprintEditorCommands::RegisterCommands`


# Variables

## Adding a new variable in BP

> [!Info] About `FEdGraphPinType` 
> In the editor the type of a variable is defined by a `FEdGraphPinType`.
> - `PinCategory` will be whatever "category" is displayed in the picker, it can be `byte` (the literal type and for enums), `int`, `int64`, `real`, `struct`, `object` (or `softobject`), `class` (or `softclass`), `interface`, ...
> - `PinSubCategory` depends, usually `None` but if selecting `float` it will be `double`
> - `PinSubCategoryObject` will hold a `UObject` weak ref to the selected object/class, if selecting a struct it will be a `UScript`

After the preliminary process mentioned above in `Bones->"+" buttons` the editor calls `FBlueprintEditor::OnAddNewVariable`. This function does the following:
- It will find a unique default var name using `FBlueprintEditorUtils::FindUniqueKismetName`.
- Then it calls `FBlueprintEditorUtils::AddMemberVariable` and passes the `UBlueprint` we are editing, the var name and the last type we used.
- And finally if it succeeds it will call `FBlueprintEditor::RenameNewlyAddedAction` manually (that's' why you can directly type to rename the variable name)

In depth of `FBlueprintEditorUtils::AddMemberVariable`:
- It calls `Modify`
- It creates a new var as a `FBPVariableDescription` and set name, GUID, property flags, and more (last pin type is either default (a boolean) or the last type set from `SBlueprintPalette::OnVarTypeChanged`).
- It adds this var in a an array named `NewVariables`.
- It calls `FBlueprintEditorUtils::ValidateBlueprintChildVariables` and `FBlueprintEditorUtils::MarkBlueprintAsStructurallyModified`. The latter calls `FBlueprintCompilationManager::CompileSynchronously` and `MarkBlueprintAsModified`.

`NewVariables` is used in A LOT of places, so here I will not cover all usages (duplicating, etc).
Here are some places where the array is used when inside `FBlueprintEditorUtils::AddMemberVariable` and after it:
- `UBlueprint::Serialize`
- `FKismetCompilerContext::CreateClassVariablesFromBlueprint`
- `FBlueprintEditorUtils::GetClassVariableList`

`FKismetCompilerContext::CreateClassVariablesFromBlueprint` will iterate the new variables and call `FKismetCompilerContext::CreateVariable` (which calls `FKismetCompilerUtilities::CreatePropertyOnScope` and `FKismetCompilerUtilities::LinkAddedProperty`) which returns a `FProperty`.

> [!Info] These functions are also used to create the variables of our components (and more?)


## Editing a variable type

The type picker is a `SBlueprintPaletteItem` (with internally `SPinTypeSelectorHelper`) (this slate widget is used in other scenarios).


When clicking the picker to open the selector `UEdGraphSchema_K2::GetVariableTypeTree` is called. This is where all types are added (one by one, or all sub types using `GatherPinsImpl::FindStructs` for structs, `GatherPinsImpl::FindObjectsAndInterfaces` for objects, classes and interfaces and `GatherPinsImpl::FindEnums` for enums)

After selecting a new type in the type picker, `SBlueprintPalette::OnVarTypeChanged` is called. Since here we are editing a BP class variable `FBlueprintEditorUtils::ChangeMemberVariableType` will be called.

Inside it will get back our var as a `FBPVariableDescription`.

For each `UBlueprint` that is a child of our Blueprint, or using it as an interface we get all `UK2Node` nodes (Get/Set nodes).
This is used later to warn the user that changing the type will break at least 1 node connection.

Then we create a new transaction and call `Modify`.
All lot of checks are run for validation and easy transfer/conversion between types.
But eventually the new type is assigned to the variable and `FBlueprintEditorUtils::MarkBlueprintAsStructurallyModified` is called on the edited BP and any BP with broken connections. `UEdGraphSchema_K2::ReconstructNode` is also called on all previously found nodes.

