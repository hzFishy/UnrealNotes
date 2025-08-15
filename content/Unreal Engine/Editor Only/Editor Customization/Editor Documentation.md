you can do custom documentation on classes and properties by using `.udn` files.
This is what is used when in the tooltip you see "Hold (Ctrl + Alt) for more".

# Resources
- See `FDocumentationModule`

# When is it displayed ?
As far I saw:
- For Types
	- Displays when you hover a class in the class picker when you create a Blueprint
	- Displays when you hover a class in the component class picker inside an actor.
- For nodes
	- Displays when you hover a node in graph editor.
# How to use
(Thanks to Hojo)

1. Get a template from `UE_5.X\Engine\Documentation\Source\Shared`. For example `UE_5.X\Engine\Documentation\Source\Shared\Types\UActorComponent\UActorComponent.INT.udn`
2. Use a toll such a VSCode to have a live markdown preview.
3. Put the file in your project/plugin `Documentation/Source/XXX` folder, if its a type the path is `Documentation/Source/Shared/UMyTypeName/UMyTypeName.INT.udn`
4. Set the content inside (use the various engine .udn files for examples)

**To add images**
Use `![](MyImage.png)`, the file must be in a `Images` folder next to your `.udn` file.

# How it works
The `FDocumentationModule` has a private var of type `IDocumentation` which seems to keep track of the `SourcePaths` as well as the `LoadedPages`.
