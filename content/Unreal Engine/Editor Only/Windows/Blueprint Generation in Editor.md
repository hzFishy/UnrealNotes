
> [!Info] About Slate classes
> Since all slate widgets are contained in `TSharedPtr`s, I won't always type `TSharedPtr<SSomeWIdget>` but `SSomeWidget`.

> [!Warning] Small disclaimer
> This topic has a LOT going on under the hood, I won't mention EVERYTHING that happens when doing X with Y, I talk about the important/relevant stuff happening.

# Related
- `USubobjectDataSubsystem`
- [[SCS & Nodes]]
- [[FBlueprintEditor]]
- [[UBlueprint & UBlueprintGeneratedClass]]

# Tabs

All the special BP tabs/windows are made in `FBlueprintEditorUnifiedMode::FBlueprintEditorUnifiedMode` (from `FBlueprintEditor::InitBlueprintEditor` -> `FBlueprintEditor::RegisterApplicationModes`)

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
> **See `UEdGraphSchema_K2` for a full and detailed list**
> In the editor the type of a variable is defined by a `FEdGraphPinType`.
> - `PinCategory` will be whatever "category" is displayed in the picker, it can be `byte` (the literal type and for enums), `int`, `int64`, `real`, `struct`, `object` (or `softobject`), `class` (or `softclass`), `interface`, ...
> - `PinSubCategory` depends, usually `None` but if selecting `float` it will be `double`
> - `PinSubCategoryObject` will hold a `UObject` weak ref to the selected object/class, if selecting a struct it will be a `UScript`

After the preliminary process mentioned above the editor calls `FBlueprintEditor::OnAddNewVariable`. This function does the following:
- It will find a unique default var name using `FBlueprintEditorUtils::FindUniqueKismetName`.
- Then it calls `FBlueprintEditorUtils::AddMemberVariable` and passes the `UBlueprint` we are editing, the var name and the last type we used.
- And finally if it succeeds it will call `FBlueprintEditor::RenameNewlyAddedAction` manually (that's' why you can directly type to rename the variable name)

**In depth of `FBlueprintEditorUtils::AddMemberVariable`:**
- It calls `Modify`
- It creates a new var as a `FBPVariableDescription` and set name, GUID, property flags, and more (last pin type is either default (a boolean) or the last type set from `SBlueprintPalette::OnVarTypeChanged`).
- It adds this var in a an array named `NewVariables`.
- It calls `FBlueprintEditorUtils::ValidateBlueprintChildVariables` and `FBlueprintEditorUtils::MarkBlueprintAsStructurallyModified`. The latter calls `FBlueprintCompilationManager::CompileSynchronously` and `MarkBlueprintAsModified`.

`NewVariables` is used in A LOT of places, so here I will not cover all usages (duplicating, etc).
Here are some places where the array is used when inside `FBlueprintEditorUtils::AddMemberVariable` and after it:
- `UBlueprint::Serialize`
- `FKismetCompilerContext::CreateClassVariablesFromBlueprint`
- `FBlueprintEditorUtils::GetClassVariableList`

`FKismetCompilerContext::CreateClassVariablesFromBlueprint` will iterate the new variables and call **`FKismetCompilerContext::CreateVariable`** (which calls `FKismetCompilerUtilities::CreatePropertyOnScope` and `FKismetCompilerUtilities::LinkAddedProperty`) which returns a `FProperty`.

> [!Info] These functions are also used to create the variables of our components (and more?)


**More on `FKismetCompilerUtilities::CreatePropertyOnScope`:**
- It does a early check to be sure that no object on the same scope has a name equal to our property name, it will find a fixed name if necessary (see `ValidatedPropertyName` var and `FKismetCompilerUtilities::CheckPropertyNameOnScope`).
- Once we are sure to have a valid property name, we are handling special extra vars type if the property is a container (Map/Set/Array), see `NewContainerProperty` var. 
- We are also setting the value of `PropertyScope`
	- `PropertyScope` is a `FFieldVariant` because if the property is a container the scope is a `FField` (`FMapProperty`/`FSetProperty`/`FArrayProperty`) but it we are a regular property the scope is a `UStruct`.
- If our property type isn't a delegate, we call `FKismetCompilerUtilities::CreatePrimitiveProperty` and set `NewProperty` with the result
	- Depending on the `PinCategory` the property is made differently.
	- For basic types like `Int64`, `Float` or `String` its always calling the same code but using a different struct property (ex: `FInt64Property`/`FFloatProperty`/`FStrProperty`).
	- For struct type it has some extra checks but ultimately it uses `FStructProperty` and set the `Struct` member variable of the property to the `UScriptType* SubType` (casted from `PinSubCategoryObject`)
	- For an Object/Interface/SoftObject type it gets the `UClass* SubType` from `SelfClass` or from `PinSubCategoryObject` and use the following property structs: `FInterfaceProperty`/`FSoftObjectProperty`/`FWeakObjectProperty`/`FObjectProperty` (they are all derived from `FObjectPropertyBase`)
	- For an Class/SoftClass type it gets the `UClass* SubType` from `PinSubCategoryObject` and use the following property structs: `FSoftClassProperty`/`FClassProperty` 
- If we are a container, `NewProperty` is used for some shady stuff then replaced by the container property (`NewMapProperty`/`NewSetProperty`/`NewArrayProperty`)
- Returns the `NewProperty`

**More on `FKismetCompilerUtilities::LinkAddedProperty`:**
- It sets the `Next` member var of the new property to be the `ChildProperties` member var of our `UStruct*` (owner of the property, here a `UBlueprintGeneratedClass`)
- it sets the `ChildProperties` member var of the structure to the the new property (See [[FField]] and [[UStruct]] for more info on these member vars)

## Editing a variable type

> [!Info] Slate type picker
> The type picker is a `SBlueprintPaletteItem` (with internally `SPinTypeSelectorHelper`, made in `SBlueprintPaletteItem::Construct`) (this slate widget is used in other scenarios).

When clicking the picker to open the selector `UEdGraphSchema_K2::GetVariableTypeTree` is called. This is where all types are added (one by one, or all sub types using `GatherPinsImpl::FindStructs` for structs, `GatherPinsImpl::FindObjectsAndInterfaces` for objects, classes and interfaces and `GatherPinsImpl::FindEnums` for enums)

After selecting a new type in the type picker, `SPinTypeSelectorHelper::OnVarTypeChanged` is called. Since here we are editing a BP class variable `FBlueprintEditorUtils::ChangeMemberVariableType` will be called.

Inside it will get back our var as a `FBPVariableDescription`.

For each `UBlueprint` that is a child of our Blueprint, or using it as an interface we get all `UK2Node` nodes (Get/Set nodes).
This is used later to warn the user that changing the type will break at least 1 node connection.

Then we create a new transaction and call `Modify`.
All lot of checks are run for validation and easy transfer/conversion between types.
But eventually the new type is assigned to the variable and `FBlueprintEditorUtils::MarkBlueprintAsStructurallyModified` is called on the edited BP and any BP with broken connections. `UEdGraphSchema_K2::ReconstructNode` is also called on all previously found nodes.


# Components

The components are shown in a `SSubobjectEditor`

## Creation (`GEN_VARIABLE`)
When you open a BP, `ContentBrowserAssetData::EditOrPreviewAssetFileItems` will eventually use `LoadPackage` to load the BP. This will construct the object.
So here in the case of BP components, `DefaultSceneRoot_GEN_VARIABLE` and so on are made from there. 

> [!Note] Note
> The the callstack isn't 100% the same between loading the scene root component and the other child components.

## Creation (viewport)
See [[FPreviewScene]] for some details on how the components are spawned in the viewport window.

## Get selected components
From the `FBlueprintEditor`, do `BlueprintEditor->GetSubobjectEditor()->GetDragDropTree()->GetSelectedItems()`, you will have a `TArray<FSubobjectEditorTreeNodePtrType>`.

To get the "mapped" real component (the one living in the preview world) see the `#Archetype` section at [[UObject]]

## Add
When you add a component from the BP UI, `SSubobjectEditor::PerformComboAddClass` is called.
This calls `SSubobjectBlueprintEditor::AddNewSubobject` then `USubobjectDataSubsystem::AddNewSubobject` then `USimpleConstructionScript::CreateNode` where `NewObject<UActorComponent>(...)` is actually called.

## Selection
When you select a component it will call `SSubobjectEditor::OnTreeSelectionChanged`

To get selected nodes use `BlueprintEditor->GetSelectedSubobjectEditorTreeNodes`

## Remove

When you delete a component in the component tree `SSubobjectBlueprintEditor::OnDeleteNodes` is called.
This then calls `USubobjectDataSubsystem::DeleteSubobjects` and since we are a Blueprint it will do the following:
- Save the SCS state in a transaction buffer.
- Call `FBlueprintEditorUtils::RemoveVariableNodes` (since our component can also be a variable in a graph)
- Same logic for `FKismetEditorUtilities::FindAllBoundEventsForComponent`
- The real remove in SCS is `USimpleConstructionScript::RemoveNodeAndPromoteChildren` (this will eventually call `USCS_Node::RemoveChildNode`).
- The `ComponentTemplate` of the SCS Node will get renamed with a new Guid and  `_REMOVED_`.


# Preview
See also [[FPreviewScene]]

# Compiling

It looks like BP instances in the levels are reinstanced when you compile a BP.

On compiling, `FBlueprintEditor::Compile` gets called from the UI Action.
This calls `FKismetEditorUtilities::CompileBlueprint` which calls `FBlueprintCompilationManager::CompileSynchronously`. 

A bunch of stuff happens in this function, the most important part is the call  to `FBlueprintCompileReinstancer::BatchReplaceInstancesOfClass` -> `FBlueprintCompileReinstancer::ReplaceInstancesOfClass_Inner`. In the latter function is where the "old to new" object remapping is done. Its also there that any "old versions" of the actors in any opening level gets destroyed.

While in the replacement/reinstancing process, `UEditorEngine::NotifyToolsOfObjectReplacement` will be called (see `FCoreUObjectDelegates::OnObjectsReplaced`)