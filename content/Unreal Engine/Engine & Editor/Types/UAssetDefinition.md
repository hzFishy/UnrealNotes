
See `AssetDefinition.h` for more info.
Basically a `UAssetDefinition` represents an top level asset in the editor.
You can edit how its viewed and what actions they have (see `UAssetDefinition_BlueprintGeneratedClass` for a good example).

# Make custom class

Mandatory
1. Make a subclass of `UAssetDefinition` or another subclass (E.g: `UAssetDefinition_ClassTypeBase`)
2. Override `GetAssetClass` and return the class you want to support.
3. Override `GetAssetDisplayName`, `GetAssetColor` (and `OpenAssets` if you aren't sub classing `UAssetDefinitionDefault`)

Then you can override other functions depending on your needs, like `GetThumbnailBrush` or `GetIconBrush`.

**Custom icon in Content Browser ?** -> Override `GetThumbnailBrush` and `GetIconBrush` (latter can just call  `GetThumbnailBrush`, same params). See [this](https://github.com/MagForceSeven/Starfire/blob/ebceb1e4b523b9926c6e3a043b19374a439d6ea4/StarfireAssets/Source/StarfireAssetsEditor/Private/DataDefinition_AssetDefinition.cpp#L37) for an example of storing the slate brush with texture.
